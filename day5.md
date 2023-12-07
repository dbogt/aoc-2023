## [AOC2023-D05] Day 5: If You Give A Seed A Fertilizer
- Full write up to come, for now my code below

https://adventofcode.com/2023/day/5

<details>
  <summary>Part 1 Solution</summary>
   
  ```python
  #%% Source files
  import pandas as pd
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d5DemoInputs.txt", "r")
  f = open(fPath+"d5ActualInputs.txt", "r")
  inputs = f.read()
  sections = inputs.split('\n\n')
  
  #%% Part 1
  def createAlmanacDF(lines):
      rows = []
      for line in lines:
          destStart, sourceStart, rangeLen = [int(x) for x in line.split()]
          rows.append({"S_Start":sourceStart,"D_Start":destStart, "Range":rangeLen})
      df = pd.DataFrame(rows)
      df['S_End'] = df['S_Start'] + df['Range'] - 1    
      df['D_End'] = df['D_Start'] + df['Range'] - 1    
      df['Delta'] = df['D_Start'] - df['S_Start'] 
      return df
  
  almanacs = {}
  
  #Grab Seed #s
  seeds = [int(x) for x in sections[0].split(": ")[-1].split()]
  
  sectionTitles = []
  for section in sections[1:]:
      lines = section.splitlines()
      sectionTitle = lines[0].split(":")[0]
      sectionTitles.append(sectionTitle)
          
      lines = section.splitlines()[1:]
      almanacs[sectionTitle] = createAlmanacDF(lines)
  
  #%% Find Match
  #seed to soil
  def findMatch(sectionTitle, sourceLookup):
      df = almanacs[sectionTitle]
      filterDF = df[(df['S_Start']<=sourceLookup) & (df['S_End']>=sourceLookup)]
      if filterDF.shape[0] > 0:
          match = filterDF.iloc[0]['Delta'] + sourceLookup
      else:
          match = sourceLookup
      return match
  
  shortHand = ['Seed','Soil','Fert','Water','Light','Temp','Humid','Loc']
  initials = {section:-1 for section in shortHand}
  seedMap = {}
  
  for seed in seeds:
  # for seed in range(1,101):
      seedMap[seed] = {'Soil':-1, 'Fert':-1, 'Water':-1,'Light':-1,'Temp':-1,'Humid':-1,'Loc':-1}
      seedMap[seed]["Seed"] = seed
      for idx, title in enumerate(sectionTitles):
          sourceType = shortHand[idx]
          destType = shortHand[idx+1]
          sourceLookup = seedMap[seed][sourceType]
          match = findMatch(title, sourceLookup)
          seedMap[seed][destType] = match
          # print(sourceType, destType, sourceLookup, match)
          
          
  dfALL = pd.DataFrame(seedMap).T
  print("Part 1", dfALL['Loc'].min())

  ```
</details>

<details>
  <summary>Part 2 Solution</summary>
  #%% Part 2
  #new seeds and ranges
  import numpy as np
  newSeedRows = []
  for idx in range(0, len(seeds), 2):
      newSeedStart = seeds[idx]
      rangeSeed = seeds[idx+1]
      newSeedRows.append({'S_Start':newSeedStart,'Range':rangeSeed})
  
  dfSeeds = pd.DataFrame(newSeedRows)
  dfSeeds['S_End'] = dfSeeds['S_Start'] + dfSeeds['Range'] - 1
  
  dfSeeds['Range_SQRT'] = np.sqrt(dfSeeds['Range']).astype(int)
  
  seedMap2 = {}
  for rowNum in range(dfSeeds.shape[0]):
      # print(rowNum)
      seedStart = dfSeeds.iloc[rowNum]['S_Start']
      seedEnd = dfSeeds.iloc[rowNum]['S_End']
      rangeSqrt = dfSeeds.iloc[rowNum]['Range_SQRT']
      print(rowNum)
      for seed in range(seedStart, seedEnd, rangeSqrt):
          seedMap2[seed] = {'Soil':-1, 'Fert':-1, 'Water':-1,'Light':-1,'Temp':-1,'Humid':-1,'Loc':-1}
          seedMap2[seed]["Seed"] = seed
          for idx, title in enumerate(sectionTitles):
              sourceType = shortHand[idx]
              destType = shortHand[idx+1]
              sourceLookup = seedMap2[seed][sourceType]
              match = findMatch(title, sourceLookup)
              seedMap2[seed][destType] = match
  
  dfALL2 = pd.DataFrame(seedMap2).T
  
  #%% reverse engneer
  minLoc = dfALL2 ['Loc'].min()
  minLocSeed = dfALL2 ['Loc'].idxmin()
  print(minLocSeed)
  
  filterDF = dfSeeds[(dfSeeds['S_Start']<=minLocSeed) & (dfSeeds['S_End']>=minLocSeed)]
  rangeSqrt = filterDF.iloc[0]['Range_SQRT']
  
  narrowStart = max(minLocSeed - rangeSqrt, filterDF.iloc[0]['S_Start'])
  narrowEnd =min( minLocSeed + rangeSqrt, filterDF.iloc[0]['S_End'])
  rangeSqrt, narrowEnd, narrowStart, narrowEnd - narrowStart
  
  seedMap3 = {}
  for seed in range(narrowStart, narrowEnd):
      seedMap3[seed] = {'Soil':-1, 'Fert':-1, 'Water':-1,'Light':-1,'Temp':-1,'Humid':-1,'Loc':-1}
      seedMap3[seed]["Seed"] = seed
      for idx, title in enumerate(sectionTitles):
          sourceType = shortHand[idx]
          destType = shortHand[idx+1]
          sourceLookup = seedMap3[seed][sourceType]
          match = findMatch(title, sourceLookup)
          seedMap3[seed][destType] = match
          
  dfALL3 = pd.DataFrame(seedMap3).T
  print("Part 2", dfALL3['Loc'].min(), dfALL3['Loc'].idxmin(), minLocSeed)
  
  dfSeeds['Range'].sum()
  ```
</details>


