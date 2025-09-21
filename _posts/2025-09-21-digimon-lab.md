---
layout: post
title: Digimon Lab
author: Oliver Tong
---

The average speed of all Digimons is 120.40160642570281.
```python
def average_digimon(stat):
    sum = 0
  
    for digimon in digimons:
        sum += digimons[digimon][stat]

    return sum / len(digimons)
```
For each Digimon, the function adds that Digimon's stat value to a sum variable, then divides it by the total number of Digimons to get the average.


This function counts the numnber of Digimon that have a specific attribute
```python
def count_digimon(column, attribute):
    count = 0
    
    for digimon in digimons:
        if digimons[digimon][column] == attribute:
            count += 1
            
    return count
```


A team with Ogremon and Kuwagamon has a memory of 14 and an attack of 308.
```python
#forms a team of up to 3 digimons that meet the memory 
#and minimum attack requiremnt
def form_team_digimon(memory, min_atk):
    team = []
    
    #sort digimons by Atk
    #starting with teams that already meet the attack requirement
    #means that less loops will be needed
    sorted_digimons = dict(sorted(digimons.items(), 
                                  key=lambda item: item[1]["Atk"], 
                                  reverse=True))
    
    #gets rid of digimons whose memory is greater than the max memory 
    copy_sorted_digimons = sorted_digimons.copy()
    for digimon in copy_sorted_digimons:
        if sorted_digimons[digimon]["Memory"] > memory:
            del sorted_digimons[digimon]
            
    #checks the total memory of the current team
    def total_memory():        
        return sum(sorted_digimons[d]["Memory"] for d in team)
        
    #check the total attack of the current team
    def total_attack():
        return sum(sorted_digimons[d]["Atk"] for d in team)
    
    #loop through every digimon as the first team member
    #if it meets the memory and attack requiremnts, return it
    for digimon1 in sorted_digimons:        
        team.append(digimon1)
        if total_memory() <= memory and total_attack() >= min_atk:
            return team

        #loop through every digimon and try to add it as the second team member
        #skip if it is the same as the first digimon
        #if it meets the memory and attack requiremnts, return the team
        for digimon2 in sorted_digimons:
            if digimon2 == digimon1:
                continue
            team.append(digimon2)
            if total_memory() <= memory and total_attack() >= min_atk:
                return team
            
            #loop through every digimon and try to add it as the third team member
            #skip if it is the same as the first digimon or second digimon
            #if it meets the memory and attack requiremnts, return the team
            for digimon3 in sorted_digimons:
                if (digimon3 == digimon1 or digimon3 == digimon2):
                    continue
                team.append(digimon3)
                if total_memory() <= memory and total_attack() >= min_atk:
                    return team
                
                
                #everytime a digimon doesn't work, remove it from the team
                team.pop()  
                  
            team.pop()
            
        team.pop()
        
    #no team found
    return None
```
This function was the most challenging to create since I had to figure out a way to form combinations of Digimon teams without using itertools. I had the most success with three nested for-loops, with each for-loop adding a new team member. The function first sorts the Digimon by their attack from highest to lowest. This means that the for-loops will begin by creating teams that already meet the attack requiremnt and will only have to focus on the memory. The function then removes any Digimon that have a memory higher than what is allowed. This reduces the amount of unnecessary loops. In the first for-loop, the first digimon is added to the team. The function checks if it meets the memory and attack requirement, and if it does, it returns the team. The other two for-loops do basically the same thing. If a digimon does not work, it is removed from the team and the function tries the next one until it finds a team.

 

