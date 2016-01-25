This is a tried and tested cross platform fixed point maths library.

It has been used in projects such as:
  * [Dingoo SDK](http://code.google.com/p/dingoo-sdk)
  * AstroLander (Included in [Dingoo SDK](http://code.google.com/p/dingoo-sdk))
  * [FGL: Flatmush/Fixed-point Graphics Library](http://code.google.com/p/fgl)

This library implements the math.h functions in fixed point (16.16) format, a subset of the currently supported functions are:
  * sin, cos, tan
  * asin, acos, atan, atan2
  * sqrt, exp
  * mul, div
  * sadd, ssub, smul, sdiv (saturated arithmetic)

I related library libfixmatrix which utilises libfixmath for matrix operations can be found here: https://github.com/PetteriAimonen/libfixmatrix