# Performance benchmarks #
The benchmarks below are shown in different configurations:
  * _ro_ is with rounding and overflow detection enabled.
  * _nn_ is `-DFIXMATH_NO_OVERFLOW -DFIXMATH_NO_ROUNDING`, ie. both disabled.

The number after the configuration means the wordwidth setting, which does not affect accuracy but is selected to be suitable for the platform:
  * _64_ is default
  * _32_ is with `-DFIXMATH_NO_64BIT`
  * _08_ is with `-DFIXMATH_OPTIMIZE_8BIT`

The performance tests are performed on a quite small number of testcases, because of the memory needed to store correct answers. The average number may not be representative of the average value in a real application, but the minimum and maximum values likely are quite accurate.

The performance is compared to compiler's own 32-bit `float` implementation.

## ARM Cortex M3 ##
Using simulator qemu-system-arm version 0.15.50 and compiler Sourcery G++ Lite 2011.03-42. The values are the _number of instructions executed_, including function call and storing result to memory. Most instructions on the platform take 1 cycle to execute, with some taking 2 cycles.

Compiler options: `-mcpu=cortex-m3 -mthumb -O2`

| **Operation**     | | **min** | **avg** | **max** | **configuration** | **code size (bytes)** |
|:------------------|:|:--------|:--------|:--------|:------------------|:----------------------|
| add               | `fix16_add` |    9    |    9    |   11    |            ro64   | 26                    |
|                   | `fix16_add` |    2    |    2    |    2    |            nn64   | ~2 (inlined)          |
|                   | float add   |   22    |   50    |   63    |                   | 352                   |
| sub               | `fix16_sub` |    9    |   10    |   11    |            ro64   | 26                    |
|                   | `fix16_sub` |    2    |    2    |    2    |            nn64   | ~2 (inlined)          |
|                   | float sub   |   23    |   51    |   64    |                   | 356                   |
| mul               | `fix16_mul` |   12    |   17    |   20    |            ro64   | 76                    |
|                   | `fix16_mul` |    8    |    8    |    8    |            nn64   | 12                    |
|                   | float mul   |   23    |   31    |   36    |                   | 360                   |
| div               | `fix16_div` |   37    |   54    |   72    |            ro64   | 182                   |
|                   | `fix16_div` |   37    |   47    |   62    |            nn64   | 166                   |
|                   | float div   |   23    |  123    |  152    |                   | 310                   |
| sqrt              | `fix16_sqrt`|  142    |  169    |  225    |            ro64   | 126                   |
|                   | `fix16_sqrt`|  139    |  166    |  222    |            nn64   | 120                   |
|                   | float sqrtf |  361    |  361    |  361    |                   | 354                   |

## Atmel ATMega 128 ##
Using simulator simulavrxx git revision as of 2012-01-05 and compiler avr-gcc 4.5.3. The values are the _number of cpu cycles_, including function call and storing result to memory.

Compiler options: `-mmcu=atmega128 -O2`


| **Operation**     | | **min** | **avg** | **max** | **configuration** | **code size (bytes)** |
|:------------------|:|:--------|:--------|:--------|:------------------|:----------------------|
| add               | `fix16_add` |   49    |   52    |   60    |            ro08   | 74                    |
|                   | `fix16_add` |   14    |   14    |   14    |            nn08   | ~14 (inlined)         |
|                   | float add   |  606    |  982    | 1262    |                   | 90 (+ 800 shared fp stuff) |
| sub               | `fix16_sub` |   49    |   53    |   60    |            ro08   | 74                    |
|                   | `fix16_sub` |   14    |   14    |   14    |            nn08   | ~14 (inlined)         |
|                   | float sub   |  612    |  988    | 1231    |                   | 98                    |
| mul               | `fix16_mul` |  294    |  344    |  453    |            ro08   | 818                   |
|                   | `fix16_mul` |  205    |  257    |  379    |            nn08   | 696                   |
|                   | float mul   |  466    | 1742    | 2112    |                   | 500                   |
| div               | `fix16_div` |  171    |  752    | 1329    |            ro08   | 414                   |
|                   | `fix16_div` |  162    |  738    | 1307    |            nn08   | 382                   |
|                   | float div   |  438    | 1293    | 1370    |                   | 348                   |
| sqrt              | `fix16_sqrt`|  605    |  714    |  934    |            ro08   | 386                   |
|                   | `fix16_sqrt`|  599    |  707    |  925    |            nn08   | 368                   |
|                   | float sqrtf |  486    |  493    |  497    |                   | 144                   |