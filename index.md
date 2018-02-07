---
title: Scientifc page for SWE
---


## Seth W. Egger
Pardon me, but this site is still under construction...


How does the brain allow for complex behaviors and cognition? Several decades of research have advanced the idea that the brain develops _internal models_ – models internal to the system that allow it to control complex systems and simulate outcomes. While this framework is instrumental to understanding sensorimotor behavior, how the brain implements internal models remains quite mysterious.

Our approach is to study the brain as it takes in sensory information and updates the internal model to furnish predictions about the near future. Take, for example, a batter attepting to hit a ball in flight. Under the internal model framework, the batter's brain implements a model to simulate the ball's flight path. This simulation is continously updated by incoming visual information through the action of a state estimator and controller (Fig 1, top). The problem with studying this system is that we can't tell apart what aspects of neural signals reflect the sensory information about the ball, the simulation of the ball, or the motor commands related to hitting the ball. 

<img src="images/Physiology/Batter.png" alt="1-2-3-Go" style="width: 500px;"/>

By introducing occluders that block vision of the ball (Fig 1, bottom), we allow ourselves the opportunity to measure the neural system as it carries out the simulation. Through a detailed analysis of human and nonhuman behavior, neurophsiological recordings of groups of neurons, and modeling, we attempt to garner a better understanding the algorithms and mechansims that allow for complex behaviors.

## 1-2-Go and 1-2-3-Go
### Task and results
To bring the batter into the lab, we developed a temporal interception task (Fig 2A,B). In the task, our subjects view a series of flashes that make up a beat. Subjects view either the first two (A) or three (B) beats, and must make a movement that coincides with the final beat (which we never actually show them). Each trial, the interval between the flashes is sampled at random from a set distribution (Fig 2C). To successfully move with the beat, the subject must measure the sample interval, _t<sub>s</sub>_, and produce the same interval, _t<sub>p</sub>_, after the last flash.

<img src="images/Psychophysics/Figure1.png" alt="1-2-Go & 1-2-3-Go" style="width: 500px;"/>

The behavior of our subjects has three interesting features (Fig 3). First, _t<sub>p</sub>_ increases with _t<sub>s</sub>_, indicating that our subjects were capable of measuring the interval. Second, they exhibit a systematic biasing of responses away from veridical _t<sub>s</sub>_ (the dashed line) and toward the mean of the distribution from which they were drawn (800 ms in this case). Finally, subject behavior improved when they were given three beats (1-2-3-Go) compared to two beats (1-2-Go; Fig 3C).

<img src="images/Psychophysics/Figure2.png" alt="Behavior" style="width: 500px;"/>

How can we explain this behavior?

### Bayesian model
From a computational standpoint, the subjects have different sources of information they should leverage to optimize their behavior – prior experience on the task and therefore some knowledge about what intervals are possible and measurements of the interval between S1 and S2 (in 1-2-Go and 1-2-3-Go) and between S2 and S3 (in 1-2-3-Go). If we assume that their interval measurements aren't perfect, the subjects ought use their prior knowledge (e.g that on average the interval is 800 ms) in combination with measurements to solve the task. This approach is often called Bayesian inference. For 1-2-Go trials, Bayes inference requires internal representations of the prior distribution, p(_t<sub>s</sub>_), and the likelihood of each _t<sub>s</sub>_, given the measured interval, p(_t<sub>m<sub>1</sub></sub>_|_t<sub>s</sub>_). By multiplying these distributions, a posterior distribution can be calculated and an estimate of _t<sub>s</sub>_ after the first measurement, _t<sub>e<sub>1</sub></sub>_ (Fig 4A).

<img src="images/Psychophysics/Figure3.png" alt="Bayesian model" style="width: 500px;"/>

For 1-2-3-Go trials, the subjects can use their second measurement, _t<sub>m<sub>2</sub></sub>_, to update the posterior with the likelihood function associated with the second measurement (Fig 4C, yellow-orange). The estimate after two measurements, _t<sub>e<sub>2</sub></sub>_, then can be derrived and used to guide production behavior. It turns out that this framework explains subject behavior very well (see curves in Fig 3A,B). The prior induces the systematic biasing, and the inclusion of a second likelihood results in improved estimation of the interval.

From an algorithmic perspective, however, the Bayesian model is difficult to implement in the time-scale required of the task (less than a second every trial). The the sophsitication of the algorithm required can be appreciated by examining Fig 4B and D. These plot the effective mapping function (e.g. what is it the subject should do) given any one (B) or pair of two (D) noisy measurements. In particular, the combination of measurements in 1-2-3-Go requires an extremely nonlinear function (as indicated by the grayscale and red contours that plot the combination of measurements that give rise to the same estimate).

### EKF
A somewhat easier way to pull of this computation in real time is to maintain an estimate of the interval and update it with each new interval measurement (Fig 5). Intuitively, we can think of the current estimated time, _t<sub>e<sub>n</sub></sub>_ as a prediction of the timing of the next flash. Errors in this prediction (e.g. _x<sub>n+1</sub>_ = _t<sub>m<sub>n+1</sub></sub>_ - _t<sub>e<sub>n</sub></sub>_), can be used to update your estimate like so: _t<sub>e<sub>n+1</sub></sub>_ = _t<sub>e<sub>n</sub></sub>_ + _k<sub>n</sub>_ _x<sub>n+1</sub>_. If you choose the weight based on the relative reliability of your estimate and measurement, this algorithm corresponds to the Kalman filter (google it!). However, because of the noise properties of your brain's interval measurement system, this simple linear algorithm just isn't good enough to match Bayes (remember those curves in 4D?). By tweeking the error signal a little bit, an "Extended Kalman Filter (EKF)" alogirhtm can apporach Bayesian integration. By updating the estimate with a nonlinear function of the error, _f_(_x<sub>n</sub>_), this algorithm can be deployed to nearly match Bayes. It turns out that such an updating algorithm better explains human behaior than either Bayesian or simpler, linear models.

<img src="images/Psychophysics/Figure6.png" alt="EKF" style="width: 250px;"/>

From a broader perspective this is an interesting finding. It suggests that the brain can't perform the sophisticated operations required of the Bayesian operation in real-time. At the same time it suggests that the approximation the brain uses is to update internal estimates. Reconsidering the task of the batter in Fig 1, this finding suggests that the brain is using the simulation to make predictions about the ball's position and updating that simulation according to a nonlinear function of the prediction error.

[Please see our full publication for more in depth information.](https://www.biorxiv.org/content/early/2018/02/07/258434)

## Physiology
But how does the brain actually pull this off? To be continued...
