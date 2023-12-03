## [AOC2023-D02] Day 2: Cube Conundrum
- I was inclined to solve today's all with regex again, but ended up using a mix of regex and old fashioned Python string .split() method
- In part 1 I stored my results extracted in a pandas DataFrame which really helped for part 2

https://adventofcode.com/2023/day/2

<details>
  <summary>Part 1 Solution</summary>
  I reused the regex from Day 1 to find all the digits in a string: r'\d+'.
  Loop could have probably been more concise as a list comprehension, but went three layers deep:
  1) loop through all the individual lines of text
  - extract the game ID by splitting at the ":" and using regex to grab the digit from that section (if you want to avoide regex, could have done another split at the space and grab last element, which is he game #)
  - extract all the draws in each game by grabbing what came after the ":", and then doing one more split at the semicolons
  - before next nested loop, reset a dictionary that will keep track of the max # of cubes for each color (colorMap = {'red':0, 'green':0, 'blue':0})

  2) second loop iterates through each individual draw and does a split at the commas to break apart each cube color section
  3) third loop iterates through each pair of # of cubes/color and breaks apart the # and the color
  - if the cubeNum is greater than the max # found so far in all the draw, replace the max in the colorMap dictionary

  At the end of the loops, save the max colorMap into the games dictionary where key=Game ID, and value=the color map of max #s for each color.

  At end of Part 1 I then converted the results into a pandas DataFrame so easier to look at results in table format. I then did some quick filtering to grab the ID's of only the possible games.
  ```python
  #%% Source files
  import re
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d2DemoInputs.txt", "r")
  f = open(fPath+"d2ActualInputs.txt", "r")
  inputs = f.read()
  lines = inputs.splitlines()

  #%% Part 1
  gameIDs = []
  codes = []
  games = {}
  for line in lines:
      gameID = line.split(":")[0]
      gameID = re.findall(r'\d+', line.split(":")[0])[0]    
      draws = line.split(": ")[-1].split("; ")
      
      colorMap = {'red':0, 'green':0, 'blue':0}
      for draw in draws:
          subsets = draw.split(', ')
          for subset in subsets:
              cubeNum = int(subset.split(" ")[0])
              cubeColor = subset.split(" ")[-1]
              if cubeNum > colorMap[cubeColor]:
                  colorMap[cubeColor] = cubeNum
      games[int(gameID)] = colorMap
  
  import pandas as pd
  df = pd.DataFrame(games).T
  
  maxPossible = {'red':12, 'green':13, 'blue':14}
  possibleDF = df.copy()
  for color, maxNum in maxPossible.items():
      possibleDF = possibleDF[possibleDF[color]<=maxNum]
  print("Part 1", sum(possibleDF.index.to_list()))
  ```
</details>

<details>
  <summary>Part 2 Solution</summary>
  
  Part 2 was fairly easy since I had the results set up as a table. I looked at the original df (not the filtered one of possible games), then calculated the power column and did a quick sum.
  ```python
  #%% Part 2
  df['power'] = df['red'] * df['green'] * df['blue']
  print("Part 2", df['power'].sum())
  ```
</details>
