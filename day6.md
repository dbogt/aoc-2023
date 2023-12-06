## [AOC2023-D06] Day 6: Wait For It
- Thought it was too good to be true when I saw how short the source input was for today
- Part 1 some quick number extraction and brute force looping
- Part 2 - I won't spoil the fun, only hint I'll give before solution below is use pen and paper and maybe refresh on some grade 9 (grade 10?) math; otherwise, if you try to brute force you'll probably end up with 60 million iterations of a loop

https://adventofcode.com/2023/day/6

<details>
  <summary>Part 1 Solution</summary>
  I'm ashamed to say, but I brute forced part 1 with a nested for loop (knowing too well that it probably won't work for Part 2). I also kept track of more variables than I needed (thinking I would need them for Part 2 later). You only need to keep track of # of wins, don't need the actual winning distances in this case.
  
  ```python
  #%% Source files
  import re
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d6DemoInputs.txt", "r")
  f = open(fPath+"d6ActualInputs.txt", "r")
  inputs = f.read()
  lines = inputs.splitlines()
  
  #%% Part 1
  times = [int(x) for x in re.findall(r'\d+', lines[0])]
  dists = [int(x) for x in re.findall(r'\d+', lines[1])]
  
  winsMap = {}
  
  for idx, time in enumerate(times):
      record = dists[idx]
      waysWin = []
      distsWin = []
      for timePress in range(1, time):
          timeMove = time - timePress
          speed = timePress
          dist = speed * timeMove
          if dist > record:
              waysWin.append(timePress)
              distsWin.append(dist)
      winsMap[time] = {'PressWins':waysWin,'DistWins':distsWin,'WinsCount':len(waysWin)}
  
  import pandas as pd
  df = pd.DataFrame(winsMap).T
  print("Part 1", df['WinsCount'].product())

  ```
</details>

<details>
  <summary>Part 2 Solution</summary>
  Part 2 was fairly once you realize you can't just brute force loops, and you need to solve with simple math. This ends up being a quadratic equation from grade 9/10 math. 

  Once you use some good old fashioned pen and paper, the equation is essentially:
  $$-1* timeToPress ^ 2 + timeRace * timeToPress - recordDistance >= 0$$

  From math class we remember that quadratic formula is: $$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$
  When $a \ne 0$,and $(ax^2 + bx + c = 0)$.

  From here we have two options, actually write out this formula in Python and solve for timeToPress, OR use numpy.roots() and provide the three coefficients.

  ```python
  #%% Part 2
  import numpy as np
  time = int("".join(re.findall(r'\d+', lines[0])))
  record = int("".join(re.findall(r'\d+', lines[1])))
  pressLow = np.ceil((-1 * time + np.sqrt(time**2 - 4*record)) / -2) 
  pressHigh = int((-1 * time - np.sqrt(time**2 - 4*record)) / -2 )
  totalWays = pressHigh - pressLow + 1
  print(totalWays)

  #%% Part 2 - Solution all with numpy
  coeff = [-1, time, -1 * record]
  roots = np.roots(coeff)
  totalWays = int(np.max(roots)) - np.ceil(np.min(roots)) + 1
  print(totalWays)
  ```
  Here is also the more detailed build up to my equation:
  ```python
  # t = time of race
  # x0 = time to press 
  # x1 = time to move = t - x0
  # c = distance of race

  # x0 + x1 = t
  # speed = x0
  # distMoved = x0 * x1
  # need dist moved > dist race
  # x0 * x1 >= c
  # x0 * (t-x0) >= c
  # - x0^2 + t*x0  - c >= 0

  #quadratic formula ax2 + bx + c = 0 --> x = [-b +/- sqrt(b^2 - 4ac)]/2a
  # b = t, a = -1, c = -c
  # x0 = (-t +/- sqrt(t^2 - 4*c)) / (-2)
</details>

