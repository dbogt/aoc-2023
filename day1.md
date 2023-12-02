## [AOC2022-D01] Day 1: Calorie Counting
- nice easy one to start
- trickiest part is getting the text manipulation correctly
- my hint for this one, check my tips above on using `inputs.splitlines()`
  - however, before you do the splitlines, first do a normal python string .split() on '\n\n'; '\n' is the special symbol for the "enter character" so basically need to tell it to split at 2x Enter (ie the blank lines)
- once you have that properly split, you can do some loops to convert the text into ints and calculate the calories
- would also recommend numpy here, and borrow their `np.max()`, `np.sum()`

https://adventofcode.com/2022/day/1

<details>
  <summary>Part 1 Solution</summary>
  
  I wanted to get clever with my list comprehension, but kept it relatively simple and just did a normal for loop at one point for Part 1.
  Part 2 was pretty easy with just doing a quick .sort (built-in list method) and then slicing on the last 3 numbers.

  ```python
  #%% Source files
  import numpy as np
  fPath = "../aoc-2022-Src/"
  # f = open(fPath+"d1DemoInputs.txt", "r")
  f = open(fPath+"d1ActualInputs.txt", "r")
  inputs = f.read()

  #%% Part 1
  elves = inputs.split('\n\n')
  elves = [elf.splitlines() for elf in elves]

  calories = []
  for elf in elves:
      cals = [int(cal) for cal in elf]
      calories.append(cals)

  totalCals = [np.sum(cals) for cals in calories]
  maxCals = np.max(totalCals)
  print("Max calories:", maxCals)
  ```
</details>

<details>
  <summary>Part 2 Solution</summary>
  
  Just need to sort and then print out last 3 items in list.
  
  ```python
  #%% Part 2
  totalCals.sort()
  print(totalCals[-3:], "Sum top 3", np.sum(totalCals[-3:]))
  ```
</details>

<details>
  <summary>Part 1 "Clever" Solution</summary>
  
  Here's also the "clever" solution for part 1 that's a lot shorter using list comprehension:

  ```python
  #%% Part 1 Clever
  calories = [[int(x) for x in  elf.splitlines()] for elf in inputs.split('\n\n')]
  totalCals = [np.sum(cals) for cals in calories]
  maxCals = np.max(totalCals)
  print("Max calories:", maxCals)
  ```
</details
