# pveclib.github.io
Repository of pveclib documentation for current releases

Header files that contain useful functions leveraging the PowerISA
Vector Facilities: Vector Multimedia Extension (VMX AKA Altivec) and
Vector Scalar Extension (VSX). Larger functions like quadword multiply
and multiple quadword multiply and madd are large enough to justify
CPU specific and tuned run-time libraries. The user can choose to bind
to platform specific static archives or dynamic shared object libraries
which automatically (dynamic linking with IFUNC resolves) select the
correct implementation for the CPU it is running on.

The goal of this project to provide well crafted implementations
of useful vector and large number operations:

- Provide equivalent functions across versions of the PowerISA.
  For example the Vector Multiply-by-10 Unsigned Quadword
  operations introduced in PowerISA 3.0 (POWER9) can be implement in a
  few vector instructions on earlier PowerISA versions.
- Provide equivalent functions across versions of the compiler.
  For example builtins provided in later versions of the compiler
  can be implemented as inline functions with inline asm in earlier
  compiler versions.
- Provide higher order functions not provided directly by the PowerISA.
  For example vector SIMD implementation for ASCII `__isalpha`, etc.
  Another example full `__int128` implementations of Count Leading Zeros,
  Population Count, and Multiply.
- Provide optimized run-time libraries for quadword integer multiply
  and multi-quadword integer multiply and add.

Most PVECLIB operations are static inline implementations provided
by PVECLIB header files: The headers are organized by element type:

    vec_common_ppc.h; Typedefs and helper macros
    vec_f128_ppc.h; Operations on vector _Float128 values
    vec_f64_ppc.h; Operations on vector double values
    vec_f32_ppc.h; Operations on vector float values
    vec_int512_ppc.h; Operations on Multi-quadword integer values
    vec_int128_ppc.h; Operations on vector __int128 values
    vec_int64_ppc.h; Operations on vector long int (64-bit) values
    vec_int32_ppc.h; Operations on vector int (32-bit) values
    vec_int16_ppc.h; Operations on vector short int (16-bit) values
    vec_char_ppc.h; Operations on vector char (8-bit) values
    vec_bcd_ppc.h; Operations on vectors of Binary Code Decimal and Zoned Decimal values


PVECLIB now (v1.0.4) supports CPU tuned run-time libraries, both static archives
and dynamic (IFUNC selected) shared objects. Currently this runtime supports
the multiple quadword multiplies and add operations documented in the
vec_int512_ppc header.

The current PVECLIB implementation assumes the target supports both VMX
(Altivec) and VSX facilities. So the minimum targets are set internally
(PVECLIB_DEFAULT_CFLAG) to '-mcpu=power7' for BE and '-mcpu=power8' for LE.

The default compiler is 'gcc'. The project can be configured to use
the Clang / LLVM compiler using the CC=clang flag.
