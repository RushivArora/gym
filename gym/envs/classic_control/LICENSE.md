   ### Description

The inverted pendulum swingup problem is based on the classic problem in control theory. The system consists of a pendulum attached at one end to a fixed point, and the other end being free. The pendulum starts in a random position and the goal is to apply torque on the free end to swing it into an upright position, with its center of gravity right above the fixed point.

The diagram below specifies the coordinate system used for the implementation of the pendulum's
dynamic equations.

![Pendulum Coordinate System](./diagrams/pendulum.png)

-  `x-y`: cartesian coordinates of the pendulum's end in meters.
- `theta` : angle in radians.
- `tau`: torque in `N m`. Defined as positive _counter-clockwise_.

### Action Space

The action is a `ndarray` with shape `(1,)` representing the torque applied to free end of the pendulum.

| Num | Action | Min  | Max |
|-----|--------|------|-----|
| 0   | Torque | -2.0 | 2.0 |


### Observation Space

The observation is a `ndarray` with shape `(3,)` representing the x-y coordinates of the pendulum's free end and its angular velocity.

| Num | Observation      | Min  | Max |
|-----|------------------|------|-----|
| 0   | x = cos(theta)   | -1.0 | 1.0 |
| 1   | y = sin(angle)   | -1.0 | 1.0 |
| 2   | Angular Velocity | -8.0 | 8.0 |

### Rewards

The reward function is defined as:


r = -(theta<sup>2</sup> + 0.1theta_dt<sup>2</sup> + 0.001*torque<sup>2</sup>)


where `$\theta$` is the pendulum's angle normalized between `[-pi, pi]` (with 0 being in the upright position).
Based on the above equation, the minimum reward that can be obtained is *-(pi<sup>2</sup> + 0.1\*8<sup>2</sup> + 0.001\*2<sup>2</sup>) = -16.2736044*, while the maximum reward is zero (pendulum is
upright with zero velocity and no torque applied).

### Starting State

The starting state is a random angle in `[-pi, pi]` and a random angular velocity in `[-1,1]`.

### Episode Termination

The episode terminates at 200 time steps.

### Arguments

- `g`: acceleration of gravity measured in *(m s<sup>-2</sup>)* used to calculate the pendulum dynamics. The default value is g = 10.0 .

```
gym.make('CartPole-v1', g=9.81)
```

### Version History

* v1: Simplify the math equations, no difference in behavior.
* v0: Initial versions release (1.0.0)





### Description

This environment corresponds to the version of the cart-pole problem
described by Barto, Sutton, and Anderson in ["Neuronlike Adaptive Elements That Can Solve Difficult Learning Control Problem"](https://ieeexplore.ieee.org/document/6313077).
A pole is attached by an un-actuated joint to a cart, which moves along a
frictionless track. The pendulum is placed upright on the cart and the goal is to balance the pole by applying forces in the left and right direction on the cart.

### Action Space

The action is a `ndarray` with shape `(1,)` which can take values `{0, 1}` indicating the direction of the fixed force the cart is pushed with.

| Num | Action                 |
|-----|------------------------|
| 0   | Push cart to the left  |
| 1   | Push cart to the right |

**Note**: The velocity that is reduced or increased by the applied force is not fixed and it depends on the angle the pole is pointing. The center of gravity of the pole varies the amount of energy needed to move the cart underneath it

### Observation Space

The observation is a `ndarray` with shape `(4,)` with the values corresponding to the following positions and velocities:

| Num | Observation           | Min                  | Max                |
|-----|-----------------------|----------------------|--------------------|
| 0   | Cart Position         | -4.8                 | 4.8                |
| 1   | Cart Velocity         | -Inf                 | Inf                |
| 2   | Pole Angle            | ~ -0.418 rad (-24°)  | ~ 0.418 rad (24°)  |
| 3   | Pole Angular Velocity | -Inf                 | Inf                |

**Note:** While the ranges above denote the possible values for observation space of each element, it is not reflective of the allowed values of the state space in an unterminated episode. Particularly:
-  The cart x-position (index 0) can be take values between `(-4.8, 4.8)`, but the episode terminates if the cart leaves the `(-2.4, 2.4)` range.
-  The pole angle can be observed between  `(-.418, .418)` radians (or **±24°**), but the episode terminates if the pole angle is not in the range `(-.2095, .2095)` (or **±12°**)

### Rewards

Since the goal is to keep the pole upright for as long as possible, a reward of `+1` for every step taken, including the termination step, is alloted. The threshold for rewards is 475 for v1.

### Starting State

All observations are assigned a uniformly random value in `(-0.05, 0.05)`

### Episode Termination

The episode terminates if any one of the following occurs:
1. Pole Angle is greater than ±12°
2. Cart Position is greater than ±2.4 (center of the cart reaches the edge of the display)
3. Episode length is greater than 500 (200 for v0)

### Arguments

```
gym.make('CartPole-v1')
```

No additional arguments are currently supported.
