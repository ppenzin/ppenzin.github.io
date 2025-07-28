---
layout: post
comments: true
title: "LLMs vs RISC-V"
date:  2025-07-25 17:46:19
categories: [LLM, RISC-V]
---

Asking Large Langugage Models RISC-V questions.

Easy one - Is Zicond part of RVA23? Claude Haiku doesn't think Zicond is a valid extension, while Sonnet correctly answers that it is part of RVA23. That is understandable given timeline of these releases.

A more difficult one would be something along use of inrinsics. There is not a lot of documentation for this online, and usable information is often buried deep in developer manuals. 

For "write C code using RVV intrinsics to compute absolute integer values of elements of an array" Haiku produces this as the core of the implentation:

```
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

Rhere is not `vabs` intrinsic or instruction in RISC-V. Interestingly, Sonnet actually gets the right answer, producing both test and negate as well as `max(x, -x)` implementation.
