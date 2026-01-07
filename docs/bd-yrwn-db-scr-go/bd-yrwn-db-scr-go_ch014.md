# 13\. SQL 解析器

SQL 易于计算机解析，同时看起来像英语。

## 13.1 语法、解析器和解释器

### 计算语言树表示

查询语言是将字符串解析为树结构以进行进一步处理的语言。

示例 1：`SELECT ... FROM foo WHERE a > b AND a < c`：

```go
 select
       /   |   \
columns  table  condition
  ...     foo      and
                 /     \
                >       <
               / \     / \
              a   b   a   c
```

示例 2：表达式`a + b * c`：

```go
 +
 / \
a   *
   / \
  b   c
```

SQL 只是特定的语法；有更简单的替代方案，例如基于管道的[PRQL](https://prql-lang.org/)，或者甚至是[S 表达式](https://en.wikipedia.org/wiki/S-expression)。

S 表达式只是嵌套的括号，它是任意树结构的简单语法。如果你选择 S 表达式，可以跳过这一章，但 SQL 并不难，因为所有的事情都是通过无上递归来处理的。本章的教训也适用于大多数计算机语言。

### 通过访问树节点进行评估

`SELECT`和`UPDATE`都可以包含列上的算术表达式，这在最后一个示例中被解析为树。每个树节点是一个运算符，其子树是操作数。要评估树节点，首先评估其子树。

```go
# pseudo code
def eval(node):
    if is_binary_operator(node):
        left, right = eval(node.left), eval(node.right)
        return node.operator(left, right)
    elif is_value(node):
        return node.value
    ...
```

回顾起来，这就是为什么树相关的理由，因为树表示评估顺序。编程语言也有控制流、变量等，但一旦用树表示，其余的应该很明显。

## 13.2 查询语言规范

### 语句

这并不完全是 SQL，只是看起来相似。

```go
create table table_name (
    a type1,
    b type2,
    ...
    index (c, b, a),
    index (d, e, f),
    primary key (a, b),
);

select expr... from table_name conditions limit x, y;
insert into table_name (cols...) values (a, b, c)...;
delete from table_name conditions limit x, y;
update table_name set a = expr, b = expr, ... conditions limit x, y;
```

### 条件

如果可能，SQL 数据库将根据`WHERE`子句选择索引，并且/或者如果条件没有被索引完全覆盖，则检索和过滤行。这是自动的，无需任何直接用户控制。

在这里，我们将偏离 SQL：我们将使用单独的子句来表示索引条件和过滤条件，

1.  `INDEX BY`子句选择索引并控制排序顺序。

```go
-- one of the 3 forms
select expr... from table_name index by a = 1;
select expr... from table_name index by a > 1;
select expr... from table_name index by a > 1 and a < 5;
-- the last query in descending order
select expr... from table_name index by a < 5 and a > 1;
```

1.  然后`FILTER`子句过滤行。

```go
-- the filter condition can be arbitrary
select expr... from table_name index by condition1 filter condition2;
select expr... from table_name filter condition2;
```

这两个都是可选的。如果缺少`INDEX BY`，则选择主键。

OLTP 工作负载通常期望可预测的性能；查询计划的突然变化是生产中的风险。这就是为什么我们使索引选择明确，以避免数据库猜测。

### 表达式

一个表达式可以是……

+   列名，

+   文字值，如数字或字符串，

+   二元或一元运算符，

+   元组。

```go
a OR b
a AND b
NOT a
a = b, a < b, ...   -- comparisons
a + b, a - b
a * b, a / b
-a
```

这被表示为一个树节点。

```go
type QLNode struct {
    Type uint32 // tagged union
    I64  int64
    Str  []byte
    Kids []QLNode // operands
}
```

不同的运算符有不同的优先级（优先级），如上所述。这种复杂性在像 S 表达式这样的简单语法中被避免。但运算符优先级可以通过简单的递归来处理，正如你将看到的。

## 13.3 递归下降

### 树节点结构

每个语句被分成更小的部分，包括表达式节点`QLNode`，因此它们是组件的树。

```go
// statements: select, update, delete
type QLSelect struct {
    QLScan
    Names  []string // expr AS name
    Output []QLNode
}
type QLUpdate struct {
    QLScan
    Names  []string
    Values []QLNode
}
type QLDelete struct {
    QLScan
}
// common structure for statements: `INDEX BY`, `FILTER`, `LIMIT`
type QLScan struct {
    Table  string // table name
    Key1   QLNode // index by
    Key2   QLNode
    Filter QLNode // filter expression
    Offset int64  // limit
    Limit  int64
}
```

### 将输入拆分为更小的部分

所有解析都是自顶向下的。最顶层是一个语句，我们首先确定其类型，然后分配具体的工作到具体函数。

```go
func pStmt(p *Parser) (r interface{}) {
    switch {
    case pKeyword(p, "create", "table"):
        r = pCreateTable(p)
    case pKeyword(p, "select"):
        r = pSelect(p)
    // ...
    }
    return r
}
```

`pKeyword` 匹配并消耗输入中的关键字以确定下一个部分。看看 `pSelect`，它的 3 个部分被 3 个函数消耗。

```go
func pSelect(p *Parser) *QLSelect {
    stmt := QLSelect{}
    pSelectExprList(p, &stmt)   // SELECT xxx
    pExpect(p, "from", "expect `FROM` table")
    stmt.Table = pMustSym(p)    // FROM table
    pScan(p, &stmt.QLScan)      // INDEX BY xxx FILTER yyy LIMIT zzz
    return &stmt
}
```

`pSelectExprList` 是逗号分隔的表达式列表。每个列表项都分配给 `pSelectExpr`。逗号决定了列表的结束。

```go
func pSelectExprList(p *Parser, node *QLSelect) {
    pSelectExpr(p, node)
    for pKeyword(p, ",") {
        pSelectExpr(p, node)
    }
}
```

`pScan` 是 `SELECT` 的最后一部分。它进一步分为 3 个更小的部分。

```go
func pScan(p *Parser, node *QLScan) {
    if pKeyword(p, "index", "by") {
        pIndexBy(p, node)
    }
    if pKeyword(p, "filter") {
        pExprOr(p, &node.Filter)
    }
    node.Offset, node.Limit = 0, math.MaxInt64
    if pKeyword(p, "limit") {
        pLimit(p, node)
    }
}
```

不看每个函数，我们已经有了解析的想法：

1.  将输入拆分为越来越小的部分，直到它结束为一个运算符、一个名称或一个字面值。

1.  通过查看下一个关键字来确定下一个部分。

`SELECT` 分解成越来越小的部分。

| `SELELT a, b` | `FROM` | `foo` | `INDEX BY x` | `FILTER y` | `LIMIT z` |
| --- | --- | --- | --- | --- | --- |
| `pSelectExprList` | `pExpect` | `pMustSym` | `pScan` |
| `pSelectExpr` |  | `pIndexBy` | `pExprOr` | `pLimit` |
| `pExprOr` |  | … | … | `pNum` |
| … |  |

### 将中缀运算符转换为二叉树

`pExprOr` 解析任意表达式。将 `1+2*3-4` 转换为考虑运算符优先级的树并不明显，因为它只是一个数字和运算符的交错列表。所以让我们从一个更简单的问题开始：只有 `+` 运算符。

```go
term
term + term
term + term + term + ...
```

表达式 `left + right` 表示为：

```go
 +
   / \
left right
```

左子树也可以表示一个表达式，所以 `LL + LR + R` 变为：

```go
 +
   / \
  +   R
 / \
LL LR
```

我们可以添加更多项，它仍然是一个二叉树。伪代码：

```go
def parse_terms():
    node = parse_column()
    while consume('+'):
        right = parse_column()
        node = QLNode(type='+', kids=[node, right])
    return node
```

这由一个简单的规则描述：

```go
expr := expr + term
expr := term
```

左子规则 `expr` 可以扩展到规则本身，但右子规则 `term` 是不能进一步扩展的底部部分。

### 递归运算符优先级

下一个问题：添加 `*` 运算符。它具有更高的优先级，因此 `term` 现在通过类似的规则扩展，底部部分现在是 `factor`。

```go
expr := expr + term
expr := term
term := term * factor
term := factor
```

伪代码：

```go
def parse_terms():
    node = parse_factors()
    while consume('+'):
        right = parse_factors()
        node = QLNode(type='+', kids=[node, right])
    return node

def parse_factors():
    node = parse_column()
    while consume('*'):
        right = parse_column()
        node = QLNode(type='*', kids=[node, right])
    return node
```

2 规则的扩展可视化。

|  | a | + | b | × | c | - | d |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | term | + |  | term |  | - | term |
| 2 | factor |  | factor | × | factor |  | factor |

`OR` 运算符具有最低优先级，因此 `pExprOr` 是解析表达式的最高函数。它调用 `pExprAnd` 来处理下一个优先级，一直到底部优先级 `pExprUnop`，它调用 `pExprAtom` 来解析最底层的部分（一个名称或一个字面值）。

```go
a OR b          -- pExprOr
a AND b         -- pExprAnd
NOT a           -- pExprNot
a = b, a < b    -- pExprCmp
a + b, a - b    -- pExprAdd
a * b, a / b    -- pExprMul
-a              -- pExprUnop
```

这被称为 *递归下降*。回顾起来，它只是分而治之，其中“分”只是检查下一个关键字。
