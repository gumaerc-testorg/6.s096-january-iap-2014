---
content_type: page
description: This section provides a sample solution to Assignment 1, Problem 1.
draft: false
learning_resource_types:
- Assignments
ocw_type: CourseSection
parent_title: Assignments
parent_type: CourseSection
parent_uid: 1330c237-1da9-2343-e1c5-e39e429984f3
title: Sample Solution to Assignment 1, Problem 1
uid: 078c6a84-ee88-3afe-34f2-c31a5dcd5d65
---
« {{% resource_link "1330c237-1da9-2343-e1c5-e39e429984f3" "Back to Assignments" %}}

```plaintext
/*
PROG: floating

LANG: C

*/

#include <stdio.h>

#include <stdlib.h>

#include <stdint.h>

#include <math.h>

 

#define ABSOLUTE_WIDTH 31

#define MANTISSA_WIDTH 23

#define EXPONENT_WIDTH 8

#define EXPONENT_MASK 0xffu

#define MANTISSA_MASK 0x007fffffu

#define EXPONENT_BIAS 127

 

union float_bits {

  float f;

  uint32_t bits;

};

 

void print_float( FILE *output, float f ) {

  union float_bits t; t.f = f;

 

  uint32_t sign_bit = ( t.bits >> ABSOLUTE_WIDTH );

  uint32_t exponent = ( t.bits >> MANTISSA_WIDTH ) & EXPONENT_MASK;

  uint32_t mantissa = ( t.bits  &  MANTISSA_MASK );

 

  if( sign_bit != 0 ) {

    fprintf( output, "-" );

  }

 

  if( exponent > 2 * EXPONENT_BIAS ) {

    fprintf( output, "Inf\n" ); /* Infinity */

    return;

  } else if( exponent == 0 ) {

    fprintf( output, "0." ); /* Zero or Denormal */

    exponent = ( mantissa != 0 ) ? exponent + 1 : exponent;

  } else {

    fprintf( output, "1." ); /* Usual */

  }

 

  for( int k = MANTISSA_WIDTH - 1; k >= 0; --k ) {

    fprintf( output, "%d", ( mantissa >> k ) & 1 );

  }

 

  if( exponent != 0 || mantissa != 0 ) {

    fprintf( output, " * 2^%d\n", (int) ( exponent - EXPONENT_BIAS ) );

  }

}

 

int main() {

  FILE *input  = fopen( "floating.in",  "r" ),

       *output = fopen( "floating.out", "w" );

 

  size_t N; float f;

  fscanf( input, "%zu", &N );

 

  for( size_t i = 0; i < N; ++i ) {

    fscanf( input, "%f", &f );

    print_float( output, f );

  }

 

  fclose( input );

  fclose( output );

  return 0;

}
```

*Below is the output using the test data:*

```plaintext
floating:
1: OK [0.004 seconds] OK!
2: OK [0.004 seconds] OK!
3: OK [0.004 seconds] OK!
4: OK [0.004 seconds] OK!
5: OK [0.005 seconds] OK!
6: OK [0.004 seconds] OK!
7: OK [0.004 seconds] OK!
```

### «