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
![](Images/Screenshot (20).png)      

