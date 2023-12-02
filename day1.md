## [AOC2023-D01] Day 1: Trebuchet?!
- some regex fun for the first day
- part two was a curveball, careful with overlapping words!
- both parts could also be solved with a lot of string manipulation in Python, but would highly recommend using regex instead

https://adventofcode.com/2023/day/1

<details>
  <summary>Part 1 Solution</summary>
  
  I wanted to get clever with my list comprehension, but kept it relatively simple and just did a normal for loop at one point for Part 1. The regex for finding all digits in a string is r'\d+'.

  ```python
  #%% Source files
  import re
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d1DemoInputs.txt", "r")
  # f = open(fPath+"d1DemoInputs_pt2.txt", "r")
  f = open(fPath+"d1ActualInputs.txt", "r")
  inputs = f.read()
  lines = inputs.splitlines()

  #%% Part 1
  codes = []
  for line in lines:
      digits = re.findall(r'\d+', line)    
      digits = "".join(digits)
      calibCode = int(digits[0] + digits[-1])
      codes.append(calibCode)
  
  print("Part 1",sum(codes))
  ```
</details>

<details>
  <summary>Part 2 Solution</summary>
  
  Part 2 was tricky if you don't pay attention to some of the examples. Some of the lines will overalpping words/letters: "eightwothree". My first approach was to simply replace all instances of "one" with '1', etc., but that obviously won't work. Instead I modified the regex to find both digits and the words needed. The regex will look like this '(?=(\d|zero|one|two|three|four|five|six|seven|eight|nine))'.
  
  ```python
  #%% Part 2
  words = ['zero','one', 'two', 'three','four','five','six','seven','eight','nine']
  wordsMap = {word:str(idx) for idx, word in enumerate(words)}
  pattern = "(?=(" + "|".join(["\d"] + words) + "))"  
  # '(?=(\d|zero|one|two|three|four|five|six|seven|eight|nine))'
  
  convert_int = lambda x: int(x) if x.isdigit() else int(wordsMap[x])
  findCode = lambda x: int(str(x[0]) + str(x[-1]))
  
  codes = []
  for line in lines:
      digits = [convert_int(x) for x in re.findall(pattern, line)]
      codes.append(findCode(digits))
  print("Part 2",sum(codes))
  ```
</details>

<details>
  <summary>Part 1 "Clever" Solution</summary>
  
  Here's also the "clever" solution for part 1 that's a lot shorter using list comprehension:

  ```python
  #%% Part 1 - Clever
  allDigits = ["".join(re.findall(r'\d+', line)) for line in lines]
  codes = [int(digits[0] + digits[-1]) for digits in allDigits]
  print("Part 1",sum(codes))
  ```
</details
