# Monte-Carlo Localization (or) Probabilistic Localization   
MCL is an algorithm used to localize a robot - Given a map of environment, the algorithm estimates the location of the robot as it moves. It uses the concepts of Total Probability and Bayes Rule.    
In this lesson, we assumed a cyclic world in which there is a robot. The robot has the map of that world and is also acquired with sensors to detect the necessary parameters (in the lesson, we used colors of each grid in the map as the measurement parameter). To make things a bit more complex, the sensor can only predict this measurement parameter (colour) with a certain probability.     
In this 2D world, the robot can move right, left, up, down or just stay where it is. And the world is considered as an m X n grid with mn boxes. The robot moves in these grids as mentioned above. Each of these grids is associated with a probability value which indicates the probability of the robot being there. The robot has an initial input after which it goes into a move-sense-move cycle.     

In the final Localization problem in the lesson, we consider a cyclic 4X5 world each grid coloured either Red(R) or Green(G) which is sensed by the robot. The robot can move UP[-1,0], DOWN[1,0], RIGHT[0,1], LEFT[0,-1] or STAY[0,0]. The measurements and the respective motions are mentioned (refer to the code) and also the required probabilities.   

**4X5 Cyclic World**:          
['R','G','G','R','R']        
['R','R','G','R','R']         
['R','R','G','G','R']               
['R','R','R','R','R']            
                   
and the measurements ['G','G','G','G','G'] and the respective motions [[0,0],[0,1],[1,0],[1,0],[0,1]] ( STAY, RIGHT, DOWN, DOWN, RIGHT)      
So, we can clearly see that this motion corresponds to 1x2 -> 1x3 -> 2x3 -> 3x3 -> 3x4 i.e., the robot ends up on the 3x4 grid. If we execute the code, we can see that this grid will have the highest probability. This is the basic core of the Localization algorithm used in self-driving cars.     

[Code](https://github.com/Jayanth2209/AI-for-Robotics/blob/master/Localization%20-%20Lesson%201/MCL.py)

                  
