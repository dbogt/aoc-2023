## [AOC2023-D04] Day 4: Scratchcards
- Created dictionary maps and counters to keep track of the # of copies for part 2
https://adventofcode.com/2023/day/4

<details>
  <summary>Part 1 Solution</summary>
  
  ```python
  #%% Source files
  import re
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d4DemoInputs.txt", "r")
  f = open(fPath+"d4ActualInputs.txt", "r")
  inputs = f.read()
  lines = inputs.splitlines()

  #%% Part 1
  cardIDs = []
  codes = []
  cardMap = {}
  cardScores = {}
  
  for line in lines:
      cardID = line.split(":")[0]
      cardID = int(re.findall(r'\d+', line.split(":")[0])[0])    
      cardIDs.append(cardID)
      
      splits = line.split(": ")[-1].split(" | ")
      winningNums = [int(x) for x in splits[0].split()]
      cardNums = [int(x) for x in splits[-1].split()]
      cardWins = [x for x in cardNums if x in winningNums]
      cardMap[cardID] = cardWins
      if len(cardWins) > 0:
          score = 1 * (2 ** (len(cardWins)-1))
          print(cardID, score, cardWins)
      else:
          score = 0
      cardScores[cardID] = score
  
  print("Part 1", sum(cardScores.values()))
  ```

</details>

<details>
  <summary>Part 2 Solution</summary>

  ```python
  #%% Part 2
  cardNumWins = {key:len(val) for key, val in cardMap.items()}
  copiesCount = {key:1 for key in cardMap.keys()}    
  
  loopCounter = 0
  for cardID, numWins in cardNumWins.items():
      count = copiesCount[cardID]
      if numWins > 0:
          # newCards = [x for x in range(cardID+1, cardID+numWins+1)]
          newCards = [x for x in range(cardID+1, cardID+min(numWins,len(cardIDs)-cardID)+1)]
          for newCardID in newCards:
              copiesCount[newCardID] += count
                      
  
  print("Part 2", sum(copiesCount.values()))
  ```

</details>
