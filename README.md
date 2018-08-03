# PID study


[//]: # (Image References)

[image1]: ./docs/images/pid_2_0_0.png "manual_tuning"
[image2]: ./docs/images/pid_1_83_3_0.png "manual_tuning"
[image3]: ./docs/images/pid_1_41_7_0_006.png "manual_tuning"
[image4]: ./docs/images/pid_1_41_7_0_003.png "manual_tuning"

[image5]: ./docs/images/p_controller.png "manual_tuning"
[image6]: ./docs/images/pi_controller.png "manual_tuning"
[image7]: ./docs/images/pd_controller.png "manual_tuning"
[image8]: ./docs/images/classic_pid.png "manual_tuning"
[image9]: ./docs/images/pessen_integral_rule.png "manual_tuning"
[image10]: ./docs/images/some_overshoot.png "manual_tuning"
[image11]: ./docs/images/no_overshoot.png "manual_tuning"

## Credit
The original work is at [here](https://github.com/ivmech/ivPID). I borrow this existing implementation
and test it with some tuning procedure I have read.

## Background

There are many propose method to tune a PID controller, and here I write down my finding in following 
these procedure to tune a PID controller

1. Online trial and error method

Adopt from [link](https://myweb.ntut.edu.tw/~jcjeng/Chap6_PID%20Tuning.pdf)

Here is the procedure
   
+ Find K<sub>P</sub>: Set τ<sub>I</sub>=&#8734;, and τ<sub>D</sub>=0. Find K<sub>C</sub> such that 
the system is critically stable. Denote this value at K<sub>P, max</sub>

+  Find τ<sub>I</sub>: Set K<sub>P</sub>=0.5K<sub>P, max</sub>, and τ<sub>D</sub>=0. Find τ<sub>I</sub> such that 
the system is critically stable. Denote this value at τ<sub>I, min</sub>

+  Find τ<sub>D</sub>: Set K<sub>P</sub>=0.5K<sub>P, max</sub>, and τ<sub>I</sub>=2τ<sub>I, min</sub>.
Find τ<sub>D</sub> such that the system is critically stable. Denote this value at 
τ<sub>D, max</sub>

+ The tuning result is K<sub>P</sub>=0.5K<sub>P, max</sub>; τ<sub>I</sub>=2τ<sub>I, min</sub>; 
τ<sub>D</sub>=0.5τ<sub>D, max</sub>

Here are tuning steps

| Step | Result | 
|:-----|:-------|
|1 (K<sub>P</sub>=2; τ<sub>I</sub>=&#8734;;τ<sub>D</sub>=0) | ![alt text][image1] |
|2 (K<sub>P</sub>=1; τ<sub>I</sub>=0.012;τ<sub>D</sub>=0) | ![alt text][image2] |
|3 (K<sub>P</sub>=1; τ<sub>I</sub>=0.006;τ<sub>D</sub>=0.006) | ![alt text][image3] |
|4 (K<sub>P</sub>=1; τ<sub>I</sub>=0.006;τ<sub>D</sub>=0.003) | ![alt text][image4] |

2. Ziegler-Nicholas method

Detail please refer to [link](https://en.wikipedia.org/wiki/Ziegler%E2%80%93Nichols_method)

+ Setting K<sub>I</sub>=0, and K<sub>D</sub>=0, and find <b>ultimate gain</b> K<sub>u</sub> and 
<b>oscillation period</b>T<sub>u</sub> 

+ Setting K<sub>P</sub>, τ<sub>I</sub> and τ<sub>D</sub> according to this table


| Control type | K<sub>P</sub> | τ<sub>I</sub> | τ<sub>D</sub> |
|:-----|:-------|:-------|:-------| 
| P | 0.5K<sub>u</sub>| - | - |
| PI | 0.45K<sub>u</sub> | T<sub>u</sub>/1.2 | - |
| PD | 0.8K<sub>u</sub> | - | T<sub>u</sub>/8 |
| classic PID | 0.6K<sub>u</sub> | T<sub>u</sub>/2 | T<sub>u</sub>/8 |
| Pessen Integral Rule | 0.7K<sub>u</sub> | T<sub>u</sub>/2.5 | 3T<sub>u</sub>/20 |
| some overshoot | 0.33K<sub>u</sub> | T<sub>u</sub>/2 | T<sub>u</sub>/3 |
| no overshoot | 0.2K<sub>u</sub> | T<sub>u</sub>/2 | T<sub>u</sub>/3 |

Ziegler-Nicholas in action


| Step | Result | 
|:-----|:-------|
| Set K<sub>P</sub> st. system oscillate. K<sub>u</sub>=2/1 and T<sub>u</sub>=0.02*2 | ![alt text][image1] |
| P controller  (K<sub>P</sub>=1; K<sub>I</sub>=0; K<sub>D</sub>=0)| ![alt text][image5] |
| PI controller (K<sub>P</sub>=0.9; K<sub>I</sub>=27; K<sub>D</sub>=0)| ![alt text][image6] |
| PD controller (K<sub>P</sub>=1.6; K<sub>I</sub>=0; K<sub>D</sub>=0.008)| ![alt text][image7] |
| classic PID (K<sub>P</sub>=1.2; K<sub>I</sub>=60; K<sub>D</sub>=0.006)| ![alt text][image8] |
| Pessen Integral Rule (K<sub>P</sub>=1.4; K<sub>I</sub>=87.5; K<sub>D</sub>=0.0084)| ![alt text][image9] |
| some overshoot (K<sub>P</sub>=0.66; K<sub>I</sub>=33; K<sub>D</sub>=0.0088)| ![alt text][image10] |
| no overshoot (K<sub>P</sub>=0.4; K<sub>I</sub>=20; K<sub>D</sub>=0.00533333)| ![alt text][image11] |

## Result

1. I run it in both windows and Linux and find that the tuning value is quite difference. It may because
of the different implementation of 3rd parties packages.

