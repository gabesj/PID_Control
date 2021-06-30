### This is Gabe Johnson's completed project for the PID Control course in Udacity's Self-Driving Car Nanodegree

The original assignment repo can be found [here.](https://github.com/udacity/CarND-PID-Control-Project)

## Goal:
The goal is to steer a car through a simulator using a PID control loop.  The simulator provides data including the car's deviation from the centerline of the track.  The assignment is to implement a proportional-integral-derivative controller to calculate a steering correction factor, which is sent back to the simulator.  
For an extra challenge, the course suggests implementing a technique or coordinate ascent (also called Twiddle), which dynamically tunes the three PID coefficients to find near-optimal values.

## Method:
The program uses PID coefficients to calculate the error in the process variable using a classic PID method.  Initially, the starting point PID coefficient values were determined by trial and error.  To do so, the Proportional coefficient (which represents absolute error in position) was adjusted first until somewhat appropriate correction was observed.  Then the Differential coefficient (which represents the rate of change of error) was adjusted until further appropriate correction was observed.  Then the Integral coefficient (which represents the cumulative error) was adjusted likewise.  Once a fairly decent starting point was determined, then the coordinate ascent was applied to further tune the coefficients.
Main.cpp has a boolean variable `do_tune`.  When it is false, the program will steer the car using the declared values of the PID coefficients.  When it is true, the program will use those coefficients as a starting point, but will use the coordinate ascent method to tweak them each lap.  Throughout each lap, the error in deviation from the centerline is accumulated to judge the performance.  Then one of the PID coefficients is adjusted and another lap driven.  If the newest set of coefficients gives better results they are retained in memory, and another PID coefficient is adjusted for the next lap.  If the results are worse, the PID coefficient is adjusted in the opposite direction for the next lap.  Then the next PID coefficient is adjusted and the process repeated.  As this continues, the magnitudes of each individual PID adjustment is scaled larger if it produces better results or smaller if it does not produce better results in either the positive or negative direction.  As the program iterates through more and more laps, the magnitues of adjustment inevitably get smaller, and eventually get to a point where it is assumed that the continued improvements are nearly negligible.  At that point, the PID coefficients that produced the best performance are displayed and the simulator continues to drive the car using these tuned values.

Here is a video showing the closed loop PID steering control:
![PIDSimulation](video/PIDvid.gif)

## Instructions for Use:
You can download the Term2 Simulator which contains the PID Controller simulator [here.](https://github.com/udacity/self-driving-car-sim/releases/tag/v1.45)  If you are using a newer version of Linux, you may need to run it with the Windowed option unchecked.

Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Change the value of `do_tune` if desired
4. Compile: `cmake .. && make`
5. Run it: `./pid`. 


## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
 
