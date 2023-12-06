## [AOC2023-D03] Day 3: Gear Ratios
- I used regex again today but instead of re.findAll, I found re.finditer to be more useful
  - re.finditer allows you to extract the mathed groupings start and end index values as well, which are handy for finding the neighbors 

https://adventofcode.com/2023/day/3

<details>
  <summary>Part 1 Solution</summary>
  
  ```python
  #%% Source files
  import re
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d3DemoInputs.txt", "r")
  f = open(fPath+"d3ActualInputs.txt", "r")
  inputs = f.read()
  lines = inputs.splitlines()

  #%% Part 1
  allNums = []
  partNums = []
  notPartNums = []
  for idx, line in enumerate(lines):
      #add "edges"
      line = "." + line + "."
      
      y0 = idx - 1
      y1 = idx + 1
      # lineNums = re.findall(r'\d+', line)
      for match in re.finditer(r'\d+', line):
          num = match.group()
          allNums.append(num)
          start = match.start()
          end = match.end()
          x0 = start - 1 #one character before start of num text
          x1 = end #one char after end of num
          topLine = "." + lines[y0] + "." if y0 >=0 else "." * len(line)
          bottomLine = "." + lines[y1] + "." if y1 < len(lines) else "." * len(line)
          topSlice = topLine[x0:x1+1]
          botSlice = bottomLine[x0:x1+1]
          leftSlice = line[x0]
          rightSlice = line[x1]
          neighb = set(topSlice + botSlice + leftSlice + rightSlice)
          # print(num, neighb)
          if len(neighb) == 1 and ('.' in neighb):
              notPartNums.append(int(num))
          else:
              partNums.append(int(num))
            
  print("Part 1", sum(partNums))
  ```

</details>

<details>
  <summary>Part 2 Solution</summary>

  ```python
  #%% Part 2
  # find all gears
  gears = []
  gearsMap = {}
  #find coordinates of all gears
  for idx, line in enumerate(lines):
      for xPos, char in enumerate(line):
          if char == "*":
              gears.append((idx, xPos))
  
  #find adjacents numbers
  for gear in gears:
      gearsMap[gear] = [] #adj numbers to store for each gear
      y = gear[0] #row/line num
      x = gear[1] #x-pos in line
      y0 = max(y - 1,0) #stop at row 0
      y1 = min(y + 1, len(lines)-1) #don't go past max line #
      rowsToCheck = list(set([y0, y, y1]))
      
      for row in rowsToCheck:
          for match in re.finditer(r'\d+', lines[row]): #find all #s in a line
              num, start, end = match.group(), match.start(), match.end()
              if row == y: #current row
                  if(end == x) or (start == x+1):
                      gearsMap[gear].append(num)  
              else: # row above or below
                  if (start-1 <= x <= end) or (x-1 <= end-1 <= x+1) or (x-1 <=start<= x+1):
                      gearsMap[gear].append(num)
  
  gearRatios = []
  for key, val in gearsMap.items():
      if len(val) == 2:
          gearRatios.append(int(val[0]) * int(val[1]))
    
  print("Part 2", sum(gearRatios))
  ```

</details>
