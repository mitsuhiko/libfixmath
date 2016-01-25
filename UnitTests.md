# Unit testing #

Some of the functions have unit tests, which are included in the `unittests` folder. The tests can be run on Linux using simply `make`, which runs the test for all CompilationOptions.

Tests can also be run on other platforms by linking `fix16_unittests.c` with the `fix16` library. The unittests will output a report to stdout.

The tests perform some corner case tests with hardcoded expected results and also automated tests using an array of test numbers, comparing against the platform `double` implementation. The tests automatically take into account ie. if rounding is disabled, so they should pass in any configuration. They will however not pass on AVR, because the 32bit-only `double` there is not accurate enough (so the _reference_ results are actually wrong).