## [AOC2023-D07] Day 7: Camel Cards
- Fun times with poker hands!
- I ended up using the Counter object from collections package: https://docs.python.org/3/library/collections.html#collections.Counter
  - E.g. if you do `Counter(['b','b','a',a','a']) it will provide a dictionary where keys are the letters and vals are the count of each unique letter
  - You can then use .most_common() to sort largest to smallest counts

https://adventofcode.com/2023/day/7

<details>
  <summary>Part 1 Solution</summary>
  
  I first made a function that categorizes the hand as "Five of a kind", "Four of a kind", etc. then put the hands, bids, and hand types as columns in a DataFrame. To easily sort, I first converted all the letter cards (T, J, Q, K, A) into letters: `cardMap = {'T':'t', 'J':'u', 'Q':'x', 'K':'y', 'A':'z'}`.
  
  Then at the end when you sort the table, you can sort first on the type category rank, and then alphabetically by hand if needed.
  
  ```python
  #%% Source files
  import pandas as pd
  from collections import Counter
  
  fPath = "../aoc-2023-Src/"
  # f = open(fPath+"d7DemoInputs.txt", "r")
  f = open(fPath+"d7ActualInputs.txt", "r")
  inputs = f.read()
  
  #Replace Map for easy sort
  cardMap = {'T':'t', 'J':'u', 'Q':'x', 'K':'y', 'A':'z'}
  for key, val in cardMap.items():
      inputs = inputs.replace(key,val)
  lines = inputs.splitlines()

  
  #%% Part 1
  def pokerHand(hand):
      cards = [*hand]
      counter = Counter(cards)
      sortHand = counter.most_common()
      mostCommon = sortHand[0][-1]
      if mostCommon == 5:
          return "Five of a kind"
      elif mostCommon == 4:
          return "Four of a kind"
      elif mostCommon == 3:
          if sortHand[1][-1] == 2:
              return "Full house"
          else:
              return "Three of a kind"
      elif mostCommon == 2:
          if sortHand[1][-1] == 2:
              return "Two pair"
          else:
              return "One pair"
      else:
          return "High card"

  hands = [l.split()[0] for l in lines]
  bids = [int(l.split()[-1]) for l in lines]
  ranks = ['High card','One pair','Two pair', 'Three of a kind', 'Full house', 'Four of a kind', 'Five of a kind']
  ranksMap = {key:idx for idx, key in enumerate(ranks)}
  types = [pokerHand(hand) for hand in hands]
      
  df = pd.DataFrame({'Card':hands,'Bid':bids,'Type':types})
  df['Type Rank'] = df['Type'].map(ranksMap)
  
  df = df.sort_values(['Type Rank','Card'])
  df = df.reset_index(drop=True)
  df['Rank #'] = df.index + 1
  df['Winnings'] = df['Rank #'] * df['Bid']
  print("Part 1", df['Winnings'].sum())

  ```
</details>

<details>
  <summary>Part 2 Hints</summary>
  Part 2 was fairly easy given how I had solved Part 1 by replacing the letter cards with new letters. I just replaced my old 'u' (which was originally 'J') with the symbol '0'. I then made a new function to categorize poker hands by checking if there are any '0' in the hand string. If there are, replace with the most common character/card. Careful for hands that are full of Jacks!
</details>
<details>
  <summary>Part 2 Solution</summary>
  
  ```python
  #%% Part 2
  #Replace J (in this case u) with '0'
  def pokerHand_pt2(hand):
      cards = [*hand]    
      counter = Counter(cards)
      if "0" in hand and counter['0'] == 5:
          return "Five of a kind"
      if "0" in hand: #joker
          cardsNoJ = [*hand.replace("0","")]    
          counterNoJ = Counter(cardsNoJ)
          bestCard = counterNoJ.most_common()[0][0]
          hand = hand.replace("0",bestCard)
          cards = [*hand]    
          counter = Counter(cards)
      return pokerHand(hand)
  
  hands2 = [hand.replace('u','0') for hand in hands]
  types2 = [pokerHand_pt2(hand) for hand in hands2]
  df2 = pd.DataFrame({'Card':hands2,'Bid':bids,'Type':types2})
  df2['Type Rank'] = df2['Type'].map(ranksMap)
  
  df2 = df2.sort_values(['Type Rank','Card'])
  df2 = df2.reset_index(drop=True)
  df2['Rank #'] = df2.index + 1
  df2['Winnings'] = df2['Rank #'] * df2['Bid']
  print("Part 2", df2['Winnings'].sum())

  ```
</details>

