---
title: Genetic Algorithm - N Queens Problem
description: "Solving the N Queens problem using a genetic algorithm."
publishDate: September 19 2023
tags: ["python", "algorithm", "genetic", "demo"]
draft: false
---

## Demo

Check out the N Queens puzzle demo [**here**](/programs/genetic-algorithm)!

## Introduction

The N Queens problem is the challenge of placing `n` chess queens on a `n x n` chessboard so that no two queens threaten each other. Traditionally methods such as backtracking algorithms are used to solve this problem, but genetic algorithms offer a fascinating alternative approach. In this post, we will delve into the intricacies of solving the N Queens problem using a genetic algorithm.

## N Queens Problem

What is the essence of the N Queens problem? Given an `n x n` chessboard, we need to place `n` queens on the board in such a way that no two queens share the same row, column, or diagonal. This puzzle becomes increasingly complex as `n` grows larger and larger.

## Genetic Algorithms

Genetic Algorithms draw inspiration from the principles of natural selection and genetics to solve problems. This involves generating a population of potential solutions, evaluating their fitness, and iteratively putting them through the evolutionary process. The process involves selection, crossover, and mutation operations. This iterative refinement mimics the evolutionary process, improving the solutions over generations.

## Solution

Here's how it works:

1. **Initialization** - Begin by creating a population of boards, each with a random placement of their queens on the chessboard.
2. **Evaluation** - Evaluate the fitness of each board by counting the number of conflicts (pairs of queens threatening each other). Smaller number is better.
3. **Selection** - Find and select the most promising solutions by sorting the boards based on their fitness scores.
4. **Pairing** - Pair up boards in sets of two to create offspring solutions.
5. **Crossover** - Combine genetic material from parent pairs to create offspring solutions by applying crossover operations. Some information from both parents will be swapped.
6. **Mutation** - Randomly alter data of the offspring solutions to introduce genetic diversity into the population.
7. **Replacement** - Replace the least fit members of the population with new offspring solutions while still maintaining the population size.
8. **Termination** - Repeat the evolutionary process until a solution is found.

---

## Code

<details>
<summary>Expand for code</summary>

```python title="board.py"
# Saurav Chhapawala

import random
import numpy as np

class Board:
    def __init__(self, n):
        self.n_queen = n
        self.map = [[0 for j in range(n)] for i in range(n)]
        self.fit = 0

        for i in range(self.n_queen):
            j = random.randint(0, self.n_queen - 1)
            self.map[i][j] = 1

    def fitness(self):
        self.fit = 0
        for i in range(self.n_queen):
            for j in range(self.n_queen):
                if self.map[i][j] == 1:
                    for k in range(1, self.n_queen - i):
                        if self.map[i + k][j] == 1:
                            self.fit += 1
                        if j - k >= 0 and self.map[i + k][j - k] == 1:
                            self.fit += 1
                        if j + k < self.n_queen and self.map[i + k][j + k] == 1:
                            self.fit += 1

    def show(self):
        print(np.matrix(self.map))
        print("Fitness: ",  self.fit)

    def flip(self, i, j):
        if self.map[i][j] == 0:
            self.map[i][j] = 1
        else:
            self.map[i][j] = 0

    def get_map(self):
        return self.map

    def get_fit(self):
        return self.fit

    def getStringRep(self):
        result = ""
        for i in range(self.n_queen):
          for j in range(self.n_queen):
            if self.map[i][j] == 1:
              result += str(j + 1)
        return result

    def setBoardfromString(self, s):
        self.map = [[0 for j in range(self.n_queen)] for i in range(self.n_queen)]
        for i in range(self.n_queen):
          num = int(s[i:i+1])
          self.map[i][num - 1] = 1
```

```python title="genetic.py"
# Genetic Algorithm approach to solving the N-Queens problem
# Saurav Chhapawala

import board
import random
import time

STATE_NUM = 8

class State:
    def __init__(self):
        self.boardObj = board.Board(QUEENS_NUM)
        #set board fitness based on current state
        self.boardObj.fitness()

        self.state = self.boardObj.getStringRep()
        self.fitness = self.boardObj.get_fit()
        self.normalized = 0

    def setNorm(self, n):
        self.normalized = n

    #function to compare normalization values of States
    def __lt__(self, other):
        return self.normalized < other.normalized


print("N Queens problem solver - Genetic Algotithm")
print("Enter a value for n (default is 5):")
QUEENS_NUM = input()
if QUEENS_NUM == "":
    QUEENS_NUM = 5
else:
    QUEENS_NUM = int(QUEENS_NUM)

#generate the starting boards
boardStates = []
for i in range(STATE_NUM):
    temp = State()
    temp.boardObj.fitness()
    boardStates.append(temp)

print("Starting Boards:")
for s in boardStates:
    s.boardObj.show()

def genetic():
    run = 0
    while True:
        #solution found
        for s in boardStates:
            if s.boardObj.get_fit() == 0:
                return s, run

        fitSum = 0
        #calculate the total of the fitness values
        for s in boardStates:
            fitSum += s.boardObj.get_fit()

        #selection process
        for s in boardStates:
            temp = s.boardObj.get_fit() / fitSum
            s.setNorm(temp)
        #sort the board states ascending by normalized value
        boardStates.sort()

        #boards will get selected depending on how fit they are
        #higher fitness means higher chance of getting selected
        selection = []
        for i in range(STATE_NUM):
            r = random.random()

            if r < boardStates[0].normalized:
                selection.append(boardStates[7])
            elif r < boardStates[0].normalized + boardStates[1].normalized:
                selection.append(boardStates[6])
            elif r < boardStates[0].normalized + boardStates[1].normalized + boardStates[2].normalized:
                selection.append(boardStates[5])
            elif r < boardStates[0].normalized + boardStates[1].normalized + boardStates[2].normalized + boardStates[3].normalized:
                selection.append(boardStates[4])
            elif r < boardStates[0].normalized + boardStates[1].normalized + boardStates[2].normalized + boardStates[3].normalized + boardStates[4].normalized:
                selection.append(boardStates[3])
            elif r < boardStates[0].normalized + boardStates[1].normalized + boardStates[2].normalized + boardStates[3].normalized + boardStates[4].normalized + boardStates[5].normalized:
                selection.append(boardStates[2])
            elif r < boardStates[0].normalized + boardStates[1].normalized + boardStates[2].normalized + boardStates[3].normalized + boardStates[4].normalized + boardStates[5].normalized + boardStates[6].normalized:
                selection.append(boardStates[1])
            else:
                selection.append(boardStates[0])

        #pairing process
        pair1 = [selection[0].boardObj.getStringRep(), selection[1].boardObj.getStringRep()]
        pair2 = [selection[2].boardObj.getStringRep(), selection[3].boardObj.getStringRep()]
        pair3 = [selection[4].boardObj.getStringRep(), selection[5].boardObj.getStringRep()]
        pair4 = [selection[6].boardObj.getStringRep(), selection[7].boardObj.getStringRep()]

        tempList = []
        n = selection[0].boardObj.n_queen

        #cross over process
        #4 groups of 2 for 8 total new states
        randIndex = random.randint(1, n - 1)
        new11 = pair1[0][0:randIndex] + pair1[1][randIndex::]
        new12 = pair1[1][0:randIndex] + pair1[0][randIndex::]

        randIndex = random.randint(1, n - 1)
        new21 = pair2[0][0:randIndex] + pair2[1][randIndex::]
        new22 = pair2[1][0:randIndex] + pair2[0][randIndex::]

        randIndex = random.randint(1, n - 1)
        new31 = pair3[0][0:randIndex] + pair3[1][randIndex::]
        new32 = pair3[1][0:randIndex] + pair3[0][randIndex::]

        randIndex = random.randint(1, n - 1)
        new41 = pair4[0][0:randIndex] + pair4[1][randIndex::]
        new42 = pair4[1][0:randIndex] + pair4[0][randIndex::]

        tempList.append(new11)
        tempList.append(new12)
        tempList.append(new21)
        tempList.append(new22)
        tempList.append(new31)
        tempList.append(new32)
        tempList.append(new41)
        tempList.append(new42)

        #mutation process
        i = 0
        for ele in tempList:
            r = random.randint(0, n)
            if r != 0:
                a = list(ele)
                a[r-1] = str(random.randint(1,n))
                ele = "".join(a)
            selection[i].boardObj.setBoardfromString(ele)
            selection[i].boardObj.fitness()
            i += 1

        state = selection.copy()
        run += 1


startTime = time.time()
finalBoard, run = genetic()
endTime = time.time()

print("Ending Board:")
print("Running time:", (endTime - startTime)*1000, "ms")
print("Runs:", run)
finalBoard.boardObj.show()
```
