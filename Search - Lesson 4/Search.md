# Motion Planning:  
Robot is in a world and has to reach a goal position - The robot has to plan a path to navigate to this goal! This process of finding a path from the start location to the goal location is called "Planning"        
### Compute Cost:         
We find the "Optimal" path by computing the total cost that is required in different possible paths and choosing the Least Cost Path. (This doesn't take into account any unexpected obstacles). For example, we can assign costs to each action taken by a robot. A forward movement can be given a cost of 1 unit, whereas turns (right/left) could be given costs as required and then an optimal path could be obtained.
