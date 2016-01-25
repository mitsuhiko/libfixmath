# Fix16 datatype limits #

The maximum representable value is `32767.999 985`. The minimum value is `-32768.0`.

The minimum value is also used to represent `fix16_overflow` for overflow detection, so for some operations it cannot be determined whether it overflowed or the result was the smallest possible value. In practice this does not matter much.

The smallest unit (machine precision) of the datatype is `1/65536 = 0.000 015`.