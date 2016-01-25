# Introduction #

This page will discuss the current codesize benifits and speed benefits of libfixmath versus the standard "math.h"

# Details #

The testing was conducted using a LPC1114 ARM Cortex-m0.

See below for the example code.

math.h test = FLOAT\_TEST

complex\_karctan2 = COMPLEX\_FIXED\_TEST

fix16\_atan2 = FIXED\_TEST

|Test | Code Size| Conversion Time|
|:----|:---------|:---------------|
|base code|1741      | N/A            |
|math.h|10768     |14340           |
|complex\_karctan2|4272      |2167            |
|fix16\_atan2|4512      |3772            |

From the table the advantage of fixed point calculations can be seen.  Code size savings of roughly 5K and computation times in the range of 20% the conversion time for the math.h implementation.



---

**Example Code**
```
#ifdef FLOAT_TEST
    start = TMR_TMR32B0TC;
    float p =10000*atan2(2.14159,1.0);
    end = TMR_TMR32B0TC;
    printf("Conversion Took %d\n",end-start);    
    k = (int)p;
    printf("Got %d\n",k);
#endif

#ifdef COMPLEX_FIXED_TEST
    start = TMR_TMR32B0TC;
    fix16_t P = fix16_mul(fix16_from_int(10000),complex_karctan2(140351, 65536));    
    end = TMR_TMR32B0TC;
    printf("Conversion Took %d\n",end-start);    
    k = fix16_to_int(P);
    printf("Got %d\n",k);
#endif

#ifdef NEW_FIXED_TEST
    start = TMR_TMR32B0TC;
    fix16_t P2 = fix16_mul(fix16_from_int(10000),fix16_atan2(140351, 65536));
    end = TMR_TMR32B0TC;
    printf("Conversion Took %d\n",end-start);    
    k = fix16_to_int(P2);
    printf("Got %d\n",k);
#endif
```

---

**Complex karctan2 Code**
```
fix16_t complex_karctan2( fix16_t y , fix16_t x)
{
    fix16_t cf1 = 51471; // = 0.78539  = pi / 4
    fix16_t cf2 = 154415; // = 2.35619 = 3pi / 4

    fix16_t absy = y;
    fix16_t angle;
    if ( absy < 0)
        absy = -absy;
    if ( x>=0)
    {
        /* r = (x-absy) / ( x + absy) */
        fix16_t r = fix16_sdiv(fix16_sadd(x,-absy),
                fix16_sadd(x,absy));
        /* angle = .1963*r^3 - .9817*r + cf1 */
        angle = fix16_sadd( fix16_sadd(
                    fix16_mul(12864,fix16_mul(r,fix16_mul(r,r))),
                    fix16_mul(-64336,r)),
                cf1);
    }
    else
    {
        /* r = (x + absy) / (absy - x) */
        fix16_t r = fix16_sdiv(fix16_sadd(x,absy),
                fix16_sadd(absy,-x));
        /* angle = .1964*r^3 - .9817*r + cf2 */
        angle = fix16_sadd( fix16_sadd(
                    fix16_mul(12864,fix16_mul(r,fix16_mul(r,r))),
                    fix16_mul(-64336,r)),
                cf2);
    }
    if( y < 0)
        return (-angle);
    return angle;
```