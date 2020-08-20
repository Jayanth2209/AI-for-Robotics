# Kalman Filters   
The Kalman filter technique is one of the popular techniques to estimate the state of a system. The difference between MCL and Kalman filters is that, Kalman filters estimates a continuous state whereas in MCL (gives Multi-Modal distribution), we chopped the world into discrete spaces. Hence, the Kalman filter gives us a Uni-Modal Distribution (single peak).       

The Kalman filter obtains the location data of a certain moving point(or object) and estimates the future locations and velocities of that point based on this acquired data.    
In MCL, the probability can be visualized as a histogram with space chopped up into discrete grids. In Kalman filters, this distrubution is a Gaussian Distribution, which is continuous over space. The area under this Gaussian, (obviously) sums up to 1.     
The 1D Gaussian is characterized by two parameters - the mean(μ) and the width of the Gaussian, also called as the variance(σ²).
