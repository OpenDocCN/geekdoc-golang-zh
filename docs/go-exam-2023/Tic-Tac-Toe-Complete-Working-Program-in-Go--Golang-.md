<!--yml

分类：未分类

日期：2024-10-13 06:45:41

-->

# Tic Tac Toe 完整工作程序在 Go 语言中

> 来源：[`golangbyexample.com/tic-tac-toe-program-golang/`](https://golangbyexample.com/tic-tac-toe-program-golang/)

目录

+   概述

+   程序

+   完整工作代码：

## **概述**

让我们首先通过一个例子了解什么是井字游戏。

+   有一个 n*n 的棋盘，每个块只能在空时标记为**交叉**或**圆圈**。

+   最多两名玩家同时进行游戏，每人轮流进行。

+   第一名玩家在其回合中在棋盘的任意块上标记**交叉**。而第二名玩家在其回合中在棋盘的任意块上标记**圆圈**。

+   目标是让某一行、某一列或某一对角线的所有块都标记为任一符号，**交叉**或**圆圈**。

+   两位玩家将努力阻止对方实现这个目标。谁先实现目标，谁就获胜。

+   如果棋盘上的所有块都已填满，而没有任何玩家能够用自己的符号标记完整的行、列或对角线，那么游戏结果为平局。

+   一旦一名玩家赢得游戏，就不允许再进行更多的移动。

让我们通过一个例子来理解这个游戏。假设有一个 3*3 的网格。点（‘.’）表示一个空块。

```go
Player 1 Move with Symbol * at Position X:1 Y:1
...
.*.
...

Player 2 Move with Symbol o at Position X:1 Y:2
...
.*o
...

Player 1 Move with Symbol * at Position X:2 Y:0
...
.*o
*..

Player 2 Move with Symbol o at Position X:0 Y:2
..o
.*o
*..

Player 1 Move with Symbol * at Position X:2 Y:2
..o
.*o
*.*

Player 2 Move with Symbol o at Position X:0 Y:0
o.o
.*o
*.*

Player 1 Move with Symbol * at Position X:2 Y:1
o.o
.*o
***

First Player Win
o.o
.*o
***
```

在上述游戏中，第一名玩家获胜，因为第三行完全被符号交叉占据 – **‘*’**

## **程序**

这里是完整的工作代码

**symbol.go**

```go
package main

type Symbol uint8

const (
    Cross Symbol = iota
    Circle
    Dot
)
```

**iPlayer.go**

```go
package main

type iPlayer interface {
    getSymbol() Symbol
    getNextMove() (int, int, error)
    getID() int
}
```

**humanPlayer.go**

```go
package main

import "fmt"

var (
	MovesPlayer1 = [4][2]int{{1, 1}, {2, 0}, {2, 2}, {2, 1}}
	MovesPlayer2 = [4][2]int{{1, 2}, {0, 2}, {0, 0}, {0, 0}}
)

type humanPlayer struct {
	symbol Symbol
	index  int
	id     int
}

func (h *humanPlayer) getSymbol() Symbol {
	return h.symbol
}

func (h *humanPlayer) getNextMove() (int, int, error) {
	if h.symbol == Cross {
		h.index = h.index + 1
		return MovesPlayer1[h.index-1][0], MovesPlayer1[h.index-1][1], nil
	} else if h.symbol == Circle {
		h.index = h.index + 1
		return MovesPlayer2[h.index-1][0], MovesPlayer2[h.index-1][1], nil
	}
	return 0, 0, fmt.Errorf("Invalid Symbol")
}

func (h *humanPlayer) getID() int {
	return h.id
}
```

**computerPlayer.go**

```go
package main

type computerPlayer struct {
	symbol Symbol
	id     int
}

func (c *computerPlayer) getSymbol() Symbol {
	return c.symbol
}

func (c *computerPlayer) getNextMove() (int, int, error) {
	//To be implemented
	return 0, 0, nil
}

func (c *computerPlayer) getID() int {
	return c.id
}
```

**gameStatus.go**

```go
package main

type GameStatus uint8

const (
	GameInProgress GameStatus = iota
	GameDraw
	FirstPlayerWin
	SecondPlayerWin
)
```

**board.go**

```go
package main

import "fmt"

type board struct {
	square    [][]Symbol
	dimension int
}

func (b *board) printBoard() {
	for i := 0; i < b.dimension; i++ {
		for j := 0; j < b.dimension; j++ {
			if b.square[i][j] == Dot {
				fmt.Print(".")
			} else if b.square[i][j] == Cross {
				fmt.Print("*")
			} else {
				fmt.Print("o")
			}

		}
		fmt.Println("")
	}
}

func (b *board) markSymbol(i, j int, symbol Symbol) (bool, Symbol, error) {
	if i > b.dimension || j > b.dimension {
		return false, Dot, fmt.Errorf("index input is greater than dimension")
	}
	if b.square[i][j] != Dot {
		return false, Dot, fmt.Errorf("input square already marked")
	}
	if symbol != Cross && symbol != Circle {
		return false, Dot, fmt.Errorf("incorrect Symbol")
	}
	b.square[i][j] = symbol
	win := b.checkWin(i, j, symbol)

	return win, symbol, nil
}

func (b *board) checkWin(i, j int, symbol Symbol) bool {
	//Check Row
	rowMatch := true
	for k := 0; k < b.dimension; k++ {
		if b.square[i][k] != symbol {
			rowMatch = false
		}
	}

	if rowMatch {
		return rowMatch
	}

	//Check Row
	columnMatch := true
	for k := 0; k < b.dimension; k++ {
		if b.square[k][j] != symbol {
			columnMatch = false
		}
	}

	if columnMatch {
		return columnMatch
	}

	//Check diagonal
	diagonalMatch := false
	if i == j {
		diagonalMatch = true
		for k := 0; k < b.dimension; k++ {
			if b.square[k][k] != symbol {
				diagonalMatch = false
			}
		}
	}

	return diagonalMatch

}
```

**game.go**

```go
package main

import "fmt"

type game struct {
    board           *board
    firstPlayer     iPlayer
    secondPlayer    iPlayer
    firstPlayerTurn bool
    moveIndex       int
    gameStatus      GameStatus
}

func initGame(b *board, p1, p2 iPlayer) *game {
    game := &game{
        board:           b,
        firstPlayer:     p1,
        secondPlayer:    p2,
        firstPlayerTurn: true,
        gameStatus:      GameInProgress,
    }
    return game
}

func (g *game) play() error {
    var win bool
    var symbol Symbol
    for {
        if g.firstPlayerTurn {
            x, y, err := g.firstPlayer.getNextMove()
            if err != nil {
                return err
            }
            win, symbol, err = g.board.markSymbol(x, y, g.firstPlayer.getSymbol())
            if err != nil {
                return err
            }
            g.firstPlayerTurn = false
            g.printMove(g.firstPlayer, x, y)
        } else {
            x, y, err := g.secondPlayer.getNextMove()
            if err != nil {
                return err
            }
            win, symbol, err = g.board.markSymbol(x, y, g.secondPlayer.getSymbol())
            if err != nil {
                return err
            }
            g.firstPlayerTurn = true
            g.printMove(g.secondPlayer, x, y)
        }
        g.moveIndex = g.moveIndex + 1
        g.setGameStatus(win, symbol)
        if g.gameStatus != GameInProgress {
            break
        }
    }
    return nil
}

func (g *game) setGameStatus(win bool, symbol Symbol) {
    if win {
        if g.firstPlayer.getSymbol() == symbol {
            g.gameStatus = FirstPlayerWin
            return
        } else if g.secondPlayer.getSymbol() == symbol {
            g.gameStatus = SecondPlayerWin
            return
        }
    }
    if g.moveIndex == g.board.dimension*g.board.dimension {
        g.gameStatus = GameDraw
        return
    }
    g.gameStatus = GameInProgress
}

func (g *game) printMove(player iPlayer, x, y int) {
	symbolString := ""
	symbol := player.getSymbol()
	if symbol == Cross {
		symbolString = "*"
	} else if symbol == Circle {
		symbolString = "o"
	}
	fmt.Printf("Player %d Move with Symbol %s at Position X:%d Y:%d\n", player.getID(), symbolString, x, y)
	g.board.printBoard()
	fmt.Println("")
}

func (g *game) printResult() {
    switch g.gameStatus {
    case GameInProgress:
        fmt.Println("Game in Between")
    case GameDraw:
        fmt.Println("Game Drawn")
    case FirstPlayerWin:
        fmt.Println("First Player Win")
    case SecondPlayerWin:
        fmt.Println("Second Player Win")
    default:
        fmt.Println("Invalid Game Status")
    }
    g.board.printBoard()
}
```

**输出**

在上述程序中，我们为两位玩家固定了移动，保存在**humanPlayer.go**文件中。以下是基于这些移动的输出。

```go
Player 1 Move with Symbol * at Position X:1 Y:1
...
.*.
...

Player 2 Move with Symbol o at Position X:1 Y:2
...
.*o
...

Player 1 Move with Symbol * at Position X:2 Y:0
...
.*o
*..

Player 2 Move with Symbol o at Position X:0 Y:2
..o
.*o
*..

Player 1 Move with Symbol * at Position X:2 Y:2
..o
.*o
*.*

Player 2 Move with Symbol o at Position X:0 Y:0
o.o
.*o
*.*

Player 1 Move with Symbol * at Position X:2 Y:1
o.o
.*o
***

First Player Win
o.o
.*o
***
```

## **完整工作代码：**

这里是一个文件中的完整工作代码

**main.go**

```go
package main

import "fmt"

type Symbol uint8

const (
	Cross Symbol = iota
	Circle
	Dot
)

type GameStatus uint8

const (
	GameInProgress GameStatus = iota
	GameDraw
	FirstPlayerWin
	SecondPlayerWin
)

type iPlayer interface {
	getSymbol() Symbol
	getNextMove() (int, int, error)
	getID() int
}

var (
	MovesPlayer1 = [4][2]int{{1, 1}, {2, 0}, {2, 2}, {2, 1}}
	MovesPlayer2 = [4][2]int{{1, 2}, {0, 2}, {0, 0}, {0, 0}}
)

type humanPlayer struct {
	symbol Symbol
	index  int
	id     int
}

func (h *humanPlayer) getSymbol() Symbol {
	return h.symbol
}

func (h *humanPlayer) getNextMove() (int, int, error) {
	if h.symbol == Cross {
		h.index = h.index + 1
		return MovesPlayer1[h.index-1][0], MovesPlayer1[h.index-1][1], nil
	} else if h.symbol == Circle {
		h.index = h.index + 1
		return MovesPlayer2[h.index-1][0], MovesPlayer2[h.index-1][1], nil
	}
	return 0, 0, fmt.Errorf("Invalid Symbol")
}

func (h *humanPlayer) getID() int {
	return h.id
}

type computerPlayer struct {
	symbol Symbol
	id     int
}

func (c *computerPlayer) getSymbol() Symbol {
	return c.symbol
}

func (c *computerPlayer) getNextMove() (int, int, error) {
	//To be implemented
	return 0, 0, nil
}

func (c *computerPlayer) getID() int {
	return c.id
}

type board struct {
	square    [][]Symbol
	dimension int
}

func (b *board) printBoard() {
	for i := 0; i < b.dimension; i++ {
		for j := 0; j < b.dimension; j++ {
			if b.square[i][j] == Dot {
				fmt.Print(".")
			} else if b.square[i][j] == Cross {
				fmt.Print("*")
			} else {
				fmt.Print("o")
			}

		}
		fmt.Println("")
	}
}

func (b *board) markSymbol(i, j int, symbol Symbol) (bool, Symbol, error) {
	if i > b.dimension || j > b.dimension {
		return false, Dot, fmt.Errorf("index input is greater than dimension")
	}
	if b.square[i][j] != Dot {
		return false, Dot, fmt.Errorf("input square already marked")
	}
	if symbol != Cross && symbol != Circle {
		return false, Dot, fmt.Errorf("incorrect Symbol")
	}
	b.square[i][j] = symbol
	win := b.checkWin(i, j, symbol)

	return win, symbol, nil
}

func (b *board) checkWin(i, j int, symbol Symbol) bool {
	//Check Row
	rowMatch := true
	for k := 0; k < b.dimension; k++ {
		if b.square[i][k] != symbol {
			rowMatch = false
		}
	}

	if rowMatch {
		return rowMatch
	}

	//Check Row
	columnMatch := true
	for k := 0; k < b.dimension; k++ {
		if b.square[k][j] != symbol {
			columnMatch = false
		}
	}

	if columnMatch {
		return columnMatch
	}

	//Check diagonal
	diagonalMatch := false
	if i == j {
		diagonalMatch = true
		for k := 0; k < b.dimension; k++ {
			if b.square[k][k] != symbol {
				diagonalMatch = false
			}
		}
	}

	return diagonalMatch

}

type game struct {
	board           *board
	firstPlayer     iPlayer
	secondPlayer    iPlayer
	firstPlayerTurn bool
	moveIndex       int
	gameStatus      GameStatus
}

func initGame(b *board, p1, p2 iPlayer) *game {
	game := &game{
		board:           b,
		firstPlayer:     p1,
		secondPlayer:    p2,
		firstPlayerTurn: true,
		gameStatus:      GameInProgress,
	}
	return game
}

func (g *game) play() error {
	var win bool
	var symbol Symbol
	for {
		if g.firstPlayerTurn {
			x, y, err := g.firstPlayer.getNextMove()
			if err != nil {
				return err
			}
			win, symbol, err = g.board.markSymbol(x, y, g.firstPlayer.getSymbol())
			if err != nil {
				return err
			}
			g.firstPlayerTurn = false
			g.printMove(g.firstPlayer, x, y)
		} else {
			x, y, err := g.secondPlayer.getNextMove()
			if err != nil {
				return err
			}
			win, symbol, err = g.board.markSymbol(x, y, g.secondPlayer.getSymbol())
			if err != nil {
				return err
			}
			g.firstPlayerTurn = true
			g.printMove(g.secondPlayer, x, y)
		}
		g.moveIndex = g.moveIndex + 1

		g.setGameStatus(win, symbol)
		if g.gameStatus != GameInProgress {
			break
		}
	}
	return nil
}

func (g *game) setGameStatus(win bool, symbol Symbol) {
	if win {
		if g.firstPlayer.getSymbol() == symbol {
			g.gameStatus = FirstPlayerWin
			return
		} else if g.secondPlayer.getSymbol() == symbol {
			g.gameStatus = SecondPlayerWin
			return
		}
	}
	if g.moveIndex == g.board.dimension*g.board.dimension {
		g.gameStatus = GameDraw
		return
	}
	g.gameStatus = GameInProgress
}

func (g *game) printMove(player iPlayer, x, y int) {
	symbolString := ""
	symbol := player.getSymbol()
	if symbol == Cross {
		symbolString = "*"
	} else if symbol == Circle {
		symbolString = "o"
	}
	fmt.Printf("Player %d Move with Symbol %s at Position X:%d Y:%d\n", player.getID(), symbolString, x, y)
	g.board.printBoard()
	fmt.Println("")
}

func (g *game) printResult() {
	switch g.gameStatus {
	case GameInProgress:
		fmt.Println("Game in Between")
	case GameDraw:
		fmt.Println("Game Drawn")
	case FirstPlayerWin:
		fmt.Println("First Player Win")
	case SecondPlayerWin:
		fmt.Println("Second Player Win")
	default:
		fmt.Println("Invalid Game Status")
	}
	g.board.printBoard()
}

func main() {

	board := &board{
		square:    [][]Symbol{{Dot, Dot, Dot}, {Dot, Dot, Dot}, {Dot, Dot, Dot}},
		dimension: 3,
	}

	player1 := &humanPlayer{
		symbol: Cross,
		id:     1,
	}

	player2 := &humanPlayer{
		symbol: Circle,
		id:     2,
	}

	game := initGame(board, player1, player2)
	game.play()
	game.printResult()

}
```

**输出**

在上述程序中，我们也为两位玩家固定了移动，保存在**humanPlayer 类**中。以下是基于这些移动的输出。

```go
Player 1 Move with Symbol * at Position X:1 Y:1
...
.*.
...

Player 2 Move with Symbol o at Position X:1 Y:2
...
.*o
...

Player 1 Move with Symbol * at Position X:2 Y:0
...
.*o
*..

Player 2 Move with Symbol o at Position X:0 Y:2
..o
.*o
*..

Player 1 Move with Symbol * at Position X:2 Y:2
..o
.*o
*.*

Player 2 Move with Symbol o at Position X:0 Y:0
o.o
.*o
*.*

Player 1 Move with Symbol * at Position X:2 Y:1
o.o
.*o
***

First Player Win
o.o
.*o
***
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程详细且我们尽力用示例覆盖所有概念。本教程适合希望掌握并深入理解 Golang 的读者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


