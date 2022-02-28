
### Description

The Mountain Car MDP is a deterministic MDP that consists of a car placed stochastically
at the bottom of a sinusoidal valley, with the only possible actions being the accelerations
that can be applied to the car in either direction. The goal of the MDP is to strategically
accelerate the car to reach the goal state on top of the right hill. There are two versions
of the mountain car domain in gym: one with discrete actions and one with continuous.
This version is the one with discrete actions.

This MDP first appeared in [Andrew Moore's PhD Thesis (1990)](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-209.pdf)

```
@TECHREPORT{Moore90efficientmemory-based,
    author = {Andrew William Moore},
    title = {Efficient Memory-based Learning for Robot Control},
    institution = {University of Cambridge},
    year = {1990}
}
```

### Observation Space

The observation is a `ndarray` with shape `(2,)` where the elements correspond to the following:

| Num | Observation                                                 | Min                | Max    | Unit |
|-----|-------------------------------------------------------------|--------------------|--------|------|
| 0   | position of the car along the x-axis                        | -Inf               | Inf    | position (m) |
| 1   | velocity of the car                                         | -Inf               | Inf  | position (m) |

### Action Space

There are 3 discrete deterministic actions:

| Num | Observation                                                 | Value   | Unit |
|-----|-------------------------------------------------------------|---------|------|
| 0   | Accelerate to the left                                      | Inf    | position (m) |
| 1   | Don't accelerate                                            | Inf  | position (m) |
| 2   | Accelerate to the right                                     | Inf    | position (m) |

### Transition Dynamics:

Given an action, the mountain car follows the following transition dynamics:

*velocity<sub>t+1</sub> = velocity<sub>t</sub> + (action - 1)\* force - cos(3\*position<sub>t</sub>)\*gravity*

*position<sub>t+1</sub> = position<sub>t</sub> + velocity<sub>t+1</sub>*

where force = 0.001 and gravity = 0.0025. The collisions at either end are inelastic with the velocity set to 0 upon collision with the wall. The position is clipped to the range `[-1.2, 0.6]` and velocity is clipped to the range `[-0.07, 0.07]`.


### Reward:

The goal is to reach the flag placed on top of the right hill as quickly as possible, as such the agent is penalised with a reward of -1 for each timestep it isn't at the goal and is not penalised (reward = 0) for when it reaches the goal.

### Starting State

The position of the car is assigned a uniform random value in *[-0.6 , -0.4]*. The starting velocity of the car is always assigned to 0.

### Episode Termination

The episode terminates if either of the following happens:
1. The position of the car is greater than or equal to 0.5 (the goal position on top of the right hill)
2. The length of the episode is 200.


### Arguments

```
gym.make('MountainCar-v0')
```

### Version History

* v0: Initial versions release (1.0.0)







### Description

The Mountain Car MDP is a deterministic MDP that consists of a car placed stochastically
at the bottom of a sinusoidal valley, with the only possible actions being the accelerations
that can be applied to the car in either direction. The goal of the MDP is to strategically
accelerate the car to reach the goal state on top of the right hill. There are two versions
of the mountain car domain in gym: one with discrete actions and one with continuous.
This version is the one with continuous actions.

This MDP first appeared in [Andrew Moore's PhD Thesis (1990)](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-209.pdf)

```
@TECHREPORT{Moore90efficientmemory-based,
    author = {Andrew William Moore},
    title = {Efficient Memory-based Learning for Robot Control},
    institution = {University of Cambridge},
    year = {1990}
}
```

### Observation Space

The observation is a `ndarray` with shape `(2,)` where the elements correspond to the following:

| Num | Observation                                                 | Min                | Max    | Unit |
|-----|-------------------------------------------------------------|--------------------|--------|------|
| 0   | position of the car along the x-axis                        | -Inf               | Inf    | position (m) |
| 1   | velocity of the car                                         | -Inf               | Inf  | position (m) |

### Action Space

The action is a `ndarray` with shape `(1,)`, representing the directional force applied on the car. The action is clipped in the range `[-1,1]` and multiplied by a power of 0.0015.

### Transition Dynamics:

Given an action, the mountain car follows the following transition dynamics:

*velocity<sub>t+1</sub> = velocity<sub>t+1</sub> + force\*self.power - 0.0025\*cos(3 * position<sub>t</sub>)*

*position<sub>t+1</sub> = position<sub>t</sub> + velocity<sub>t+1</sub>*

where force is the action clipped to the range `[-1,1]` and power is a constant 0.0015. The collisions at either end are inelastic with the velocity set to 0 upon collision with the wall. The position is clipped to the range [-1.2, 0.6] and velocity is clipped to the range [-0.07, 0.07].

### Reward

A negative reward of *-0.1\*action<sup>2</sup>* is received at each timestep to penalise for taking actions of large magnitude. If the mountain car reaches the goal then a positive reward of +100 is added to the negative reward for that timestep.

### Starting State

The position of the car is assigned a uniform random value in `[-0.6 , -0.4]`. The starting velocity of the car is always assigned to 0.

### Episode Termination

The episode terminates if either of the following happens:
1. The position of the car is greater than or equal to 0.45 (the goal position on top of the right hill)
2. The length of the episode is 999.

### Arguments

```
gym.make('MountainCarContinuous-v0')
```

### Version History

* v0: Initial versions release (1.0.0)
