# Kalman Filters   
The Kalman filter technique is one of the popular techniques to estimate the state of a system. The difference between MCL and Kalman filters is that, Kalman filters estimates a continuous state whereas in MCL (gives Multi-Modal distribution), we chopped the world into discrete spaces. Hence, the Kalman filter gives us a Uni-Modal Distribution (single peak).       

The Kalman filter obtains the location data of a certain moving point(or object) and estimates the future locations and velocities of that point based on this acquired data.    
In MCL, the probability can be visualized as a histogram with space chopped up into discrete grids. In Kalman filters, this distrubution is a Gaussian Distribution, which is continuous over space. The area under this Gaussian, (obviously) sums up to 1.     
The 1D Gaussian is characterized by two parameters - the mean(μ) and the width of the Gaussian, also called as the variance(σ²). Variance is the measure of uncertainity. More is the variance, the more uncertain we are about the actual state.      

The Kalman filter iterates two different things - measurement updates and  motion updates (also called prediction). Measurement -> Motion -> Measurement cycle: similar to MCL.    
We use Bayes Rule and Total Probability for measurement and motion updates respectively.      
So, initially we have a prior Gaussian distribution (with a mean μ and variance σ² - which will be higher because we are very uncertain about the location). Now we get a measurement about the vehicle/robot localization (with a mean v and variance r² - which will be smaller because the new measurement told us quite a bit where the vehicle is). Now with this data, we get a subsequent Gaussian with a mean somewhere between prior and the measurement and with a much smaller variance(a narrow peak). This Gaussian, say has a mean μ' and σ'². These values can be given as:     
* μ' = (r²μ + σ²v)/(r² + σ²)   
* σ'² = σ²r²/(r²+σ²)      
So mathematically, we can see that μ' is between μ and v, but closer to v (since we are taking a weighted mean and σ²>r²) and also that σ'² < r² < σ²      

#### Kalman Filter Code: 
In the lesson, we saw the basic Kalman filter code that outputs the mean and variance of the final Gaussian obtained by measurement and respective motion updates.     
[Code](https://github.com/Jayanth2209/AI-for-Robotics/blob/master/Kalman%20Filters%20-%20Lesson%202/KF1.py)       

#### Multivariate Gaussians:    
These are Gaussians in multiple dimensions(i.e., more than 1 dimension).       
The mean(**μ**) now is represented as a vector with an element with each dimension        
**μ** = (μ0 μ1 .... μD)    
The variance now is replaced by what is called as covariance(**∑**) which is a matrix with D rows and D columns (dimensionality of the estimate is D). The covariance defines the spread of the gaussian.     
For example, in 2D we can represent the Gaussian with contour curves as shown in the figure below          
![](https://github.com/Jayanth2209/AI-for-Robotics/blob/master/Kalman%20Filters%20-%20Lesson%202/Images/Screenshot%20(20).png)         

Here x0,y0 pair constitute the mean(**μ**) and the spread of the lines corresponds to the covariance(**∑**). When the Gaussian is tilted as in the figure, then the uncertainity of x and y is correlated, which means if I get information about x, you can get the corresponding information about y, by following the contour line. This is called Correlation.                   

In the Kalman filter land, we build a 2D-estimate - one for the location(x) -along x axis and one for the velocity(ẋ) - along y axis. So, if initially, we know the location of the object - we represent it as a gaussian around it which is very narrow along x and broad along y(because we are fairly uncertain about the velocity).        
Now coming to the prediction step, how will we predict the future location without the velocity??...             

So, let's just pick a point on the initial Gaussian(assume velocity is zero and we are at x = 1 i.e., (1,0)). Now, assuming the velocity to be zero, where would the posterior be after the prediction??... 
It would be at the same location since the velocity is zero. Now, consider the velocity to be 1 (we are at (1,1)), now where would the posterior be??...Well, it'd be at (2,1) - because we move by 1 (1+1 = 2) and we still assume velocity to be 1. Now, if velocity was 2 - we'd be at (3,2). Now, consider doing this for various points on Gaussian and put it all together - you will get points like (1,0), (2,1), (3,2) and so on.... and we get a tilted Gaussian. This Gaussian expresses the correlation between x and ẋ - The faster I move, the further I am from my initial position.            

Now, consider the second measurement, which again tells us only about the position but not velocity. Let's say the measurement gives the position as x = 2. Now, this will be a Gaussian like the first one but around x=2. Now this is our measurement and the tilted Gaussian we obtained is our prior. Multiplying our prior with this measurement, we get a Gaussian now which has a really good estimate of what our velocity and position is (around (2,1)). Now, if we move along considering this prior, we end up at (3,2) - which is a really good estimate of our future location and velocity - This concept is the core of Kalman Filters.

The Kalman Filter States can be classified into - Observables(position/location in our example) and Hidden variables(velocity in our example). But because the Observables and the Hidden states interact/correlate, subsequent observations of the observables give us information about the hidden variables. So in layman's terms - as in the previous example - from multiple observations of the posititon of the object, we can estimate how fast it moves. 

#### Designing a Kalman Filter:     
To design a Kalman filter, we effectively need two things - for the state, you need a state transition function : State Transition Matrix.(x ẋ) --> (x ẋ). Similarly for measurement, we'll need a measurement function.    
**Example**: For the 1D motion we discussed above: **x' = x + ẋ**  and **ẋ' = ẋ**       
So the State Transition matrix would be - **F = (1 1; 0 1)** and for the measurement function matrix, we get **H = (1,0)** i.e., **z = (1,0).(x ẋ)**          

**Now, the actual update equations for a Kalman filter are:**     
There's a prediction step, where we take the best estimate **X** and multiply it with the state transition matrix **F** and we the add to it, the motion vector **U**       
* **X' = F.X + U**         

We also have a covariance that characterizes the uncertainity - **P** = Covariance Uncertainity and that is updated as follows:       
* **P' = F.P.F^(T)**                  
(F^(T) is the transpose matrix of F)           

For the Measurement update step, we use the measurement **Z** and compare it with our prediction **H.X** (**H** is the measurement function). We call their difference as the error(**Y**): **Y = Z - H.X**     
This error is mapped into a matrix **S**, which is obtained by projecting the system uncertainity into the measurement space using the measurement function projection. This is added with a matrix **R**, that characterizes the measurement noise. Therefore:       
* **S = H.P.H^(T) + R**       

This is then mapped into a variable **K**(called the Kalman Gain) as follows:      
* **K = P.H^(T).S^(-1)**               
(S^(-1) is the inverse matrix of S)         

**Then, we finally update our estimate and our uncertainity as follows:** (Phew!!)        
* **X' = X + K.Y**       
* **P' = (I - K.H).P**        

[Multi Dimensional Kalman Filter - Code](https://github.com/Jayanth2209/AI-for-Robotics/blob/master/Kalman%20Filters%20-%20Lesson%202/multiKF.py)


