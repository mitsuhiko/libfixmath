# Introduction #
The sections below document the current state of each of the major trig functions supported by the libfixmath library.  For each function a comparison to the floating point version is made.


# Sine #
The sine function is approximated by a  6th order Taylor series.  This Taylor series is most accurate between -pi and pi so before performing the approximation the input angle is transformed into the range -pi to pi.

```
angle = angle % (2 * pi)
if( angle > pi)
    angle -=  (2 * pi)
else if( angle < -pi)
    angle += (2 * pi)
```

The angle is then input into the following Taylor series.

![http://latex.codecogs.com/png.latex?\large%20sin(x)%20=%20x%20-%20\frac{x^{3}}{3!}%20+%20\frac{x^{5}}{5!}%20-%20\frac{x^{7}}{7!}%20+\frac{x^{9}}{9!}%20-%20\frac{x^{11}}{11!}%.png](http://latex.codecogs.com/png.latex?\large%20sin(x)%20=%20x%20-%20\frac{x^{3}}{3!}%20+%20\frac{x^{5}}{5!}%20-%20\frac{x^{7}}{7!}%20+\frac{x^{9}}{9!}%20-%20\frac{x^{11}}{11!}%.png)

The following graph shows the error of libfixmath sine versus the math.h implementation of sine.
From the graph it can be seen that the approximation of sine starts to become wrong for values near +-pi

![http://libfixmath.googlecode.com/svn/images/sinf-fix16_sin.png](http://libfixmath.googlecode.com/svn/images/sinf-fix16_sin.png)

Using the fixtest program the following numbers are generated (compiled with -O3)
|Processor | Float support | Fixed runtime / Float runtime | Error |
|:---------|:--------------|:------------------------------|:------|
|2GHz Core Duo | Yes           | 3.40                          | 2.11  |
|ARM Cortex-M0 | No            |.427                           | 1.95  |




## Alternate Sine ##
If you notice in the graph above the range -pi/2 to pi/2 the approximation is almost perfect approximating the sine function.  By applying some more constraints to the input angle we can constrain the calculation to the range -pi/2 to pi/2.  Using the following if block (after the previous if block) constrains the angle to a range where we can drop the Taylor series down to a 4th order.

```
if (angle > (pi / 2))
    angle = pi - angle;
else if (angle < -(pi / 2))
    angle = -pi - angle;
```

The angle is then input into the following Taylor series.

![http://latex.codecogs.com/png.latex?\large%20sin(x)%20=%20x%20-%20\frac{x^{3}}{3!}%20+%20\frac{x^{5}}{5!}%20-%20\frac{x^{7}}{7!}%20%.png](http://latex.codecogs.com/png.latex?\large%20sin(x)%20=%20x%20-%20\frac{x^{3}}{3!}%20+%20\frac{x^{5}}{5!}%20-%20\frac{x^{7}}{7!}%20%.png)

The following graph shows the error for this implementation versus the math.h sine approximation.  The approximation still begins to vary around the +-pi/2 value but the error is very small.  for improved accuracy the 6th order Taylor series could be used with the above constraints

![http://libfixmath.googlecode.com/svn/images/sinf-fix16altsin.png](http://libfixmath.googlecode.com/svn/images/sinf-fix16altsin.png)

Using the fixtest program the following numbers are generated (compiled with -O3)
|Processor | Float support | Fixed runtime / Float runtime | Error |
|:---------|:--------------|:------------------------------|:------|
|2GHz Core Duo | Yes           | 2.52                          | 2.11  |
|ARM Cortex-M0 | No            | .332                          | 2.24  |

On the Core duo this method runs in 76% of the time compared to the 6th order taylor series bounded to +-pi with less than .01% error difference

On the ARM Cortex-M0 this method runs in 77% of the time compared to the 6th order taylor series bounded to +-pi with less than .19% error difference.


---


# Cosine #
The Cosine is calculated using the following identity.  Since the approximation is based off the Sine calculation the error and speed versus the math.h cosine should be largely the same.

![http://latex.codecogs.com/png.latex?\large%20cos(x)%20=%20sin(x+\frac{\pi%20}{2})%.png](http://latex.codecogs.com/png.latex?\large%20cos(x)%20=%20sin(x+\frac{\pi%20}{2})%.png)

|Processor | Float support | Fixed runtime / Float runtime | Error |
|:---------|:--------------|:------------------------------|:------|
|2GHz Core Duo | Yes           | 3.48                          | 2.10  |
|ARM Cortex-M0| No            | .429                          | 2.25  |


---


# Tangent #
The Tangent is calculated using the following identity. Since the approximation is based off the Sine calculation the error and speed versus the math.h cosine should be largely the same.

![http://latex.codecogs.com/png.latex?\large%20tan(x)%20=%20\frac{sin(x)}{cos(x)}%.png](http://latex.codecogs.com/png.latex?\large%20tan(x)%20=%20\frac{sin(x)}{cos(x)}%.png)

The Tangent function could use some work.  When either sin(x) or cos(x) approaches 0 the error gets huge... I limited this graph to +-1.5
![http://libfixmath.googlecode.com/svn/images/tanf-fix16_tan.png](http://libfixmath.googlecode.com/svn/images/tanf-fix16_tan.png)

TODO
|Processor | Float support | Fixed runtime / Float runtime | Error |
|:---------|:--------------|:------------------------------|:------|
|2GHz Core Duo | Yes           |                               |       |
|ARM Cortex-M0| No            |                               |       |


---

# ArcSine #


---

# ArcCosine #


---

# ArcTangent #
The ArcTangent is calculated using the approximation found at http://www.dspguru.com/dsp/tricks/fixed-point-atan2-with-self-normalization. And optimized by flatmush


|Processor | Float support | Fixed runtime / Float runtime | Error |
|:---------|:--------------|:------------------------------|:------|
|2GHz Core Duo | Yes           | 3.47                          | .01%  |
|ARM Cortex-M0| No            | .623                          | .004% |


## Alternate ArcTangent ##
Using this approximation no optimization of the code was done

![http://libfixmath.googlecode.com/svn/images/atanf-fix16_atan.png](http://libfixmath.googlecode.com/svn/images/atanf-fix16_atan.png)

|Processor | Float support | Fixed runtime / Float runtime | Error |
|:---------|:--------------|:------------------------------|:------|
|2GHz Core Duo | Yes           | 3.23                          | .01%  |
|ARM Cortex-M0| No            | .330                          | .004% |