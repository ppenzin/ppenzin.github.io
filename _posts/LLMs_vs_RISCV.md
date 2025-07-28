---
layout: post
comments: true
title: "LLMs vs RISC-V"
date:  2025-07-25 17:46:19
categories: [LLM, RISC-V]
---

First exposuore to rudimentary ML technologies is GEHL branch predictor that I've done for a class project. It is of course light years away from the machine learning tech that is all the talk this days, but it is also solving a problem that is light years away from the problems AI is used for today.

Asking Large Langugage Models RISC-V questions.

Easy one - Is Zicond part of RVA23? Claude Haiku doesn't think Zicond is a valid extension, while Sonnet correctly answers that it is part of RVA23. That is understandable given timeline of these releases.

A more difficult one would be something along use of inrinsics. There is not a lot of documentation for this online, and usable information is often buried deep in developer manuals. 

For "write C code using RVV intrinsics to compute absolute integer values of elements of an array" Haiku produces this as the core of the implentation:

```C
void compute_abs_int_vector(int* input, int* output, size_t n) {
    // Determine maximum vector length
    size_t vlen = __riscv_vsetvlmax_e32m1();
    
    // Process elements in vector chunks
    for (size_t i = 0; i < n; i += vlen) {
        // Determine number of elements to process in this iteration
        size_t vlmax = __riscv_vsetvl_e32m1(n - i, vlen);
        
        // Load vector of integers
        vint32m1_t vec_input = __riscv_vle32_v_i32m1(input + i, vlmax);
        
        // Compute absolute value using vector instructions
        vint32m1_t vec_abs = __riscv_vabs_v_i32m1(vec_input, vlmax);
        
        // Store result back to output array
        __riscv_vse32_v_i32m1(output + i, vec_abs, vlmax);
    }
}
```

There is no `vabs` intrinsic or instruction in RISC-V. Interestingly, Sonnet actually gets the right answer, producing two alternative implementations:

```C
// Function to compute absolute values using RVV intrinsics
void compute_abs_rvv(const int32_t* input, int32_t* output, size_t n) {
    size_t vl;
    
    // Process the array in chunks based on vector length
    for (size_t i = 0; i < n; i += vl) {
        // Set vector length for this iteration
        vl = __riscv_vsetvl_e32m1(n - i);
        
        // Load vector from input array
        vint32m1_t vec_input = __riscv_vle32_v_i32m1(input + i, vl);
        
        // Compute absolute value
        // Method 1: Using conditional operations
        vbool32_t mask_negative = __riscv_vmslt_vx_i32m1_b32(vec_input, 0, vl);
        vint32m1_t vec_neg = __riscv_vneg_v_i32m1(vec_input, vl);
        vint32m1_t vec_abs = __riscv_vmerge_vvm_i32m1(vec_input, vec_neg, mask_negative, vl);
        
        // Store result to output array
        __riscv_vse32_v_i32m1(output + i, vec_abs, vl);
    }
}

// Alternative implementation using vmax
void compute_abs_rvv_alt(const int32_t* input, int32_t* output, size_t n) {
    size_t vl;
    
    for (size_t i = 0; i < n; i += vl) {
        vl = __riscv_vsetvl_e32m1(n - i);
        
        // Load vector from input array
        vint32m1_t vec_input = __riscv_vle32_v_i32m1(input + i, vl);
        
        // Compute absolute value using max(x, -x)
        vint32m1_t vec_neg = __riscv_vneg_v_i32m1(vec_input, vl);
        vint32m1_t vec_abs = __riscv_vmax_vv_i32m1(vec_input, vec_neg, vl);
        
        // Store result to output array
        __riscv_vse32_v_i32m1(output + i, vec_abs, vl);
    }
}
```

This is actually quite impressive, as onlie resources discussing this can be counted on one hand - meanning the model was able to correcty abosrb this tiny moresl of intformation in its ocean of traning data.

It doesn't correctly descibe the tradeoffs between the two.
