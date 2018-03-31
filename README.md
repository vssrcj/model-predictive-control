# Car Mobile Predictive Control (MPC)

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

*made by [CJ](https://github.com/vssrcj)*

# Overview.

The goal of this program is to navigate a track provided by [this](https://github.com/udacity/self-driving-car-sim/releases) simulator.

The solution is made with C++, with the use of IPOPT and CPPAD libraries.  It fits a third-degree polynomial dynamically to the given waypoints over a relatively short distance.

---

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

# Model

The kinematic model includes the vehicle's:
* `x` and `y` coordinates
* orientation angle / heading direction (`psi`)
* velocity
* cross-track error (`cte`) and psi error (`epsi`).

`Lf` is defined as the distance between the car's front and rear wheels.

The actuator outputs are:
* acceleration (`a`)
* steering angle (`delta`)

The model combines the state and actuations from the previous timestep to calculate the next state based on the following functions:

<div>
  <img src="/images/modal.png" height="500">
</div>

The objective is to find the `a` and the `delta` that minimizes the errors (based on various factors).

# Timestep Length and Elapsed Duration

The number of points(N) and the time interval(dt) define the prediction horizon, which is set to 10 and 0.1 respectively.
This means that the prediction polynomial is fitted to these N points, every dt second.
If you increase N, or decrease dt, the performance descreases, and the inverse leads to erratic behaviour.

# Polynomial Fitting and MPC Preprocessing

The waypoints are pre-processed by transforming them to the vehicle's perspective.  This puts the vehicle's x and y position at the origin (0, 0), and the orientation angle at 0.

A trajectory is then predicted, and a third-degree polynomial is fitted to the transformed waypoints.  

# Model Predictive Control with Latency

The original kinematic equations depend on the previous actuation's timestep.  With a delay, these actuations can only be applied another timestep later, so the model is altered to account for this.
