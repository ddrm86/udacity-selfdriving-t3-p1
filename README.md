## Path Planning Project
### David del Río Medina

---

## [Rubric](https://review.udacity.com/#!/rubrics/1020/view) Points

#### 1. The code compiles correctly.

The code compiles with no errors using `cmake` and `make`.

#### 2. The car is able to drive at least 4.32 miles without incident.

Although the actual version is not 100% incidents free, several test performed show that the car is able to drive without any incident farther than the mandatory distance. 

#### 3. The car drives according to the speed limit.

The car is able to maintain a fixed reference speed of 49.5 mph unless there are obstacles ahead of the road (lines `382-383`).

#### 4. Max Acceleration and Jerk are not Exceeded.

The car never changes its reference speed by more than 5 m/s in every cycle (lines `378-384`).

Trajectories are smoothed by using splines (lines `453-456`) and the waypoints are spaced taking into account the current reference velocity (lines `465-481`). Lane changing trajectories generate curves that are shallow enough to avoid excessive jerk (lines `427-431`).

#### 5. Car does not have collisions.

The car checks if the vehicle ahead in the same lane is too close, and tries to change lanes or reduce the speed if necessary. The car only tries to change lanes if there is space enough to avoid a collision with the vehicles right behind or ahead in the lane (lines `322-385`).

With the current version, the car is sometimes involved in a collision when other vehicles in the road make a sudden change (hard acceleration or braking, quick lane change).

#### 6. The car stays in its lane, except for the time between changing lanes.

The car is able to stay in one of the three lanes. Lane changes are smooth, but quick enough (lines `427-431`).

#### 7. There is a reflection on how to generate paths.

The path generation code is based on the project walkthrough published by Udacity.

The basic idea is to use reference points from the previous path and the future path (where we want to go) to generate a spline that smoothly connects sequential trajectories. The actual path waypoints given to the system are sampled from the generated spline, with a spacing that keeps the reference speed while avoiding jumps in the acceleration and jerk.

1. Two of the reference points are the last two points from the previous path that the car has not finished following yet. If there is no previous path, the current car position is used. Lines `394-425`.

2. The other three reference points are located 30, 60 and 90 meters ahead of the current car position, in the center of the lane where we want the car to be, based on the path planning algorithm. Lines `427-439`.

3. The reference points are converted to local car coordinates to ease calculations, e.g. avoiding vertical splines that could give more than one `y` coordinate for the same `x`. Lines `442-451`.

4. A spline is generated with the five reference points. Lines `453-456`.

5. At the end of every cycle, we give the simulator a total of 50 path points. The previous path points that the car has not followed yet are given again to the simulator. Lines `458-563`.

6. The rest of the path points, up to 50, are generated by sampling from the spline. The sample path points are spaced by taking into account the reference speed, avoiding violating maximum acceleration and jerk. Before adding them to the path, the points are converted back to global map coordinates. Lines `465-492`.
