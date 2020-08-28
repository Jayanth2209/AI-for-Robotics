# Particle Filters
* Easiest to program       
* Flexible     

##### Properties:     
* Continuous State Space    
* Multimodal Distributions     
* Are also approximate like Histogram and Kalman Filters     

##### Introduction:   
As before, the robot has to perform what's called as 'global localization' i.e., the robot is completely uncertain of where it is, and has to localize ltself (based on sensor measurements). Particle filters represent this uncertainity (or certainity) with particles. Each particle represents a discrete guess, where the robot might be - It is structured as an X-coordinate, Y-coordinate and also a heading direction - these three values together comprise a single guess. So, a set os several such guesses comprise an approximate representation for the posterior of the robot and thereby comprise the filter.       
In the beginning, all the particles are uniformly distributed. But the particle filter makes them survive, in proportion of how consistent one of these particles is with a sensor measurement. Slowly, the filter eliminates the particle clouds where the robot is certainly not there and eventually the robot localizes itself i.e., the robot is now aware of where it is. This basically implies that, the particles which are more consistent with the measurement are the most likely to survive, and as a result places with high probability collect more particles, and therefore will be more representative of the robot's posterior belief.                

##### Example:     
We will consider a robot in a 100x100 cyclic world, with four landmarks. The robot can turn and move in the 2D world. The robot can sense the distance to four designated landmarks (L1, L2, L3 and L4). These distances comprise the measurement vector of the robot.    
The particle filter that we will program maintains a set of (N = 1000) particles i.e., 1000 random guesses  as to where the robot might be. Each of these dots/particles is a vector containing the X-coordinate, Y-coordinate and a heading direction, as mentioned before. These particles are randomly generated.        

Consider a particle that is away from the actual position of the robot and also has a different heading direction. Now, when we take the measurement vector and apply to this particle, it will obviously differ from the actual measurement vector of that particle i.e., the sensor data of distances to L1, L2, L3 and L4 obtained from the measurement will not coincide with the actual distances of "this" particle from the landmarks. Hence, the location of this particle will become really unlikely.      
The closer our particle is to the actual position, the more likely will be the set of measurements i.e., the mismatch of the actual and predicted measurement will be low.    

This is the trick in particle filters - The difference/mismatch between the actual measurement and predicted measurement gives rise to what is called the "importance weight", which tells us how important/how likely a specific particle is. Larger the weight, more important it is. So, in the code we implement, the probability of survival of particles with larger importance weight will be more than that of the lesser weight particles.       

Now, we do what's called as "Resampling" - randomly drawing N new particles from the old particles with replacement in proportion to their importance weight. After the resampling, the more plausible particles live on and the least plausible particles will probably die out. This is what we have discussed in the "Introduction" section.     

So, we need to implement a method of assigning importance weights and also to implement resampling. After assigning the importance weights, we perform resampling. In resampling, we construct a new set of N particles from the old set. Now, these elements of the newly constructed set are selected randomly. The particle with higher importance weight will be more likely to be selected than that with a lesser importance weight. For example, let's say Pk (k-th particle) has a large importance weight, it is likely to be selected more number of times i.e., the new set of N particles would be more likely to contain more than one Pk. And similarly, if a particle Pj has a very low importance weight, it is very less likely for it to get selected into the new set, and even if it does, it is very less likely that it would get selected more than once.       
###### Note: Before resampling, the importance weights are normalized       
These normalized importance weights will be the probability of the particle,for being picked in the resampling. There is even a really minute chance that the particle with the "highest" probabilty is not even picked in the resampling - but again, this is a very unlikely event, but it isn't impossible.        
[Refer to the video lectures for explanation on Resampling Wheel](https://classroom.udacity.com/courses/cs373/lessons/48704330/concepts/487480820923)         


