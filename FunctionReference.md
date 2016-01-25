# Q16.16 Fixed point functions #

These functions operate on 32-bit numbers, `fix16_t`, which have 16 bit integer part and 16 bit fractional part.

## Conversion functions ##
Conversion from integers and floating point values. These conversions retain the numeric value and perform rounding where necessary.

  * `fix16_from_int(a)` Simply multiplies a by `fix16_one = 65536`
  * `fix16_to_int(a)` Divides by `fix16_one` and rounds to nearest integer.
  * `fix16_from_float(a)` Multiplies by `fix16_one` and rounds to nearest value.
  * `fix16_to_float(a)` Divides by `fix16_one`. Rounding is according to the current floating point mode.
  * `fix16_from_dbl(a)` Multiplies by `fix16_one` and rounds to nearest value.
  * `fix16_to_dbl(a)` Divides by `fix16_one`. All `fix16_t` values fit into a `double`, so no rounding happens.
  * `fix16_from_str(buf)` Converts from string to `fix16_t`.
  * `fix16_to_str(value, buf, decimals)` Converts from `fix16_t` to string.

Accuracy: all non-trivial ones are covered by tests and accurate up to datatype limits.

## Basic arithmetic ##
These functions perform rounding and detect overflows, unless disabled with CompilationOptions. When overflow is detected, they return `fix16_overflow` as a marker value.

  * `fix16_add(a,b)` Addition
    * Implementation 1: just C's + operator
    * Implementation 2: with overflow detection
  * `fix16_sub(a,b)` Subtraction.
    * Implementation 1: just C's - operator
    * Implementation 2: with overflow detection
  * `fix16_mul(a,b)` Multiplication .
    * Implementation 1: using `int32_t * int32_t -> int64_t`
    * Implementation 2: using `(u)int16_t * (u)int16_t -> (u)int32_t`
    * Implementation 3: using `uint8_t * uint8_t -> int16_t`
  * `fix16_div(a,b)` Division.
    * Implementation 1: using `uint32_t / uint32_t`
    * Implementation 2: performing division manually with `uint32_t`

Accuracy: these functions are covered by UnitTests and are accurate to Fix16Limits.

## Saturating arithmetic ##
Functions for basic arithmetic that saturate instead of overflowing. They are based around the functions listed above, but add extra logic to convert `fix16_overflow` to either `fix16_min` or `fix16_max`.

  * `fix16_sadd(a,b)` Saturating addition
  * `fix16_ssub(a,b)` Saturating subtraction
  * `fix16_smul(a,b)` Saturating multiplication
  * `fix16_sdiv(a,b)` Saturating division

Accuracy: these functions are not yet covered by tests.

## Transcendental functions ##
Roots, exponents & similar.

  * `fix16_sqrt(a)` Square root. Performs rounding and is accurate to Fix16Limits.
  * `fix16_exp(a)` Exponential function using power series approximation. Accuracy depends on range, worst case +-40 absolute for negative inputs and +-0.003% for positive inputs. Average error is +-1 for neg and +-0.0003% for pos.
  * `fix16_log(a)` Natural logarithm using Newton approximation and fix16\_exp(). Worst case error +-3 absolute, average error less than 1 unit.

These functions are covered by UnitTests.

## Trigonometric functions ##
Functions for sin, tan etc.

  * `fix16_sin(a)` Sine for angle in radians
  * `fix16_sin_parabola(a)` Alternative sine implementation.
  * `fix16_cos(a)` Cosine for angle in radians
  * `fix16_tan(a)` Tangent for angle in radians
  * `fix16_asin(a)` Inverse of sine, output in radians
  * `fix16_acos(a)` Inverse of cosine, output in radians
  * `fix16_atan(a)` Inverse of tangent, output in radians
  * `fix16_atan2(a,b)` Inverse of tangent with automatic sign handling. Output in radians.

Accuracy: these functions are not yet covered by tests.