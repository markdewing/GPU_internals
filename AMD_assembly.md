# AMD GPU assembly

ISA Manuals: https://rocmdocs.amd.com/en/latest/GCN_ISA_Manuals/GCN-ISA-Manuals.html

PDF versions and CPU manuals: https://developer.amd.com/resources/developer-guides-manuals/

Top level of ROCm documentation: https://rocmdocs.amd.com/en/latest/

Video on [Understanding AMD GPU ISA](https://www.youtube.com/watch?v=HYrs_TGWgz4) (from [NERSC GPUs for Science 2020](https://www.nersc.gov/users/training/gpus-for-science/gpus-for-science-2020/)).   This video walks step by step through the generated assembly for an AXPY kernel. 


### Vector add example

```
#define RealType float

__global__ void vector_add(const RealType *a, const RealType *b, RealType *c, const int N)
{
  int idx = blockDim.x * blockIdx.x + threadIdx.x;
  if (idx < N) {
    c[idx] = a[idx] + b[idx];
  }
}
```

Using `hipcc -v -save-temps=cwd -O2 -amdgpu-target=gfx900 hip_vector_add.cpp` leaves a number of intermediate files.
The device assembly is in `hip_vector_add-hip-amdgcn-amd-amdhsa-gfx900.s`

See the "Understanding AMD GPU ISA" video above for a detailed walkthrough of a similar kernel.

```
  .protected  _Z10vector_addPKfS0_Pfi ; -- Begin function _Z10vector_addPKfS0_Pfi
  .globl  _Z10vector_addPKfS0_Pfi
  .p2align  8
  .type _Z10vector_addPKfS0_Pfi,@function
_Z10vector_addPKfS0_Pfi:                ; @_Z10vector_addPKfS0_Pfi
_Z10vector_addPKfS0_Pfi$local:
; %bb.0:
  s_load_dword s0, s[6:7], 0x18
  s_load_dword s1, s[4:5], 0x4
  s_waitcnt lgkmcnt(0)
  s_and_b32 s1, s1, 0xffff
  s_mul_i32 s8, s8, s1
  v_add_u32_e32 v0, s8, v0
  v_cmp_gt_i32_e32 vcc, s0, v0
  s_and_saveexec_b64 s[0:1], vcc
  s_cbranch_execz BB0_2
; %bb.1:
  s_load_dwordx4 s[0:3], s[6:7], 0x0
  s_load_dwordx4 s[4:7], s[6:7], 0x10
  v_ashrrev_i32_e32 v1, 31, v0
  v_lshlrev_b64 v[0:1], 2, v[0:1]
  s_waitcnt lgkmcnt(0)
  v_mov_b32_e32 v5, s3
  v_mov_b32_e32 v3, s5
  v_add_co_u32_e32 v2, vcc, s4, v0
  v_addc_co_u32_e32 v3, vcc, v3, v1, vcc
  v_add_co_u32_e32 v4, vcc, s2, v0
  v_addc_co_u32_e32 v5, vcc, v5, v1, vcc
  v_mov_b32_e32 v6, s1
  v_add_co_u32_e32 v0, vcc, s0, v0
  v_addc_co_u32_e32 v1, vcc, v6, v1, vcc
  global_load_dword v0, v[0:1], off
  global_load_dword v1, v[4:5], off
  s_waitcnt vmcnt(0)
  v_add_f32_e32 v0, v0, v1
  global_store_dword v[2:3], v0, off
BB0_2:
  s_endpgm

```
