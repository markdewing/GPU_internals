# Intel Gen Assembly

Introduction: https://software.intel.com/content/www/us/en/develop/articles/introduction-to-gen-assembly.html

Detailed documentation: https://01.org/linuxgraphics/documentation

## Vector add example

OpenCL kernel
```
__kernel void vector_add(__global const float *a, __global const float *b, __global float *c)
{
  int id = get_global_id(0);

  c[id] = a[id] + b[id];
}
```

The [Compute Runtime](https://github.com/intel/compute-runtime) (NEO) contains an offload compiler (`ocloc`).   
To get an assembly listing, it appears the OpenCL kernel first should be compiled, then disassembled.
The compile step:
```
ocloc -file kernel_vector_add.cl -device cfl
```
This creates three files with `.bin`, `.gen`, and `.spv` suffixes.
Now disassembly:
```
ocloc disasm -device cfl -file kernel_vector_add_Gen9core.bin
```
By default the output is in a directory named `dump`.  The `vector_add_KernelHeap.asm` file is

```
L0:
(W)     mov (8|M0)               r3.0<1>:ud    r0.0<1;1,0>:ud                  
(W)     or (1|M0)                cr0.0<1>:ud   cr0.0<0;1,0>:ud   0x4C0:uw              {Switch}
(W)     mul (1|M0)               r6.0<1>:d     r9.1<0;1,0>:d     r3.1<0;1,0>:d   
(W)     mov (8|M0)               r127.0<1>:ud  r3.0<8;8,1>:ud                   {Compacted}
        add (16|M0)              r4.0<1>:d     r6.0<0;1,0>:d     r1.0<16;16,1>:uw
        add (16|M16)             r10.0<1>:d    r6.0<0;1,0>:d     r2.0<16;16,1>:uw
        add (16|M0)              r4.0<1>:d     r4.0<8;8,1>:d     r7.0<0;1,0>:d    {Compacted}
        add (16|M16)             r10.0<1>:d    r10.0<8;8,1>:d    r7.0<0;1,0>:d   
        shl (16|M0)              r12.0<1>:d    r4.0<8;8,1>:d     2:w              
        shl (16|M16)             r14.0<1>:d    r10.0<8;8,1>:d    2:w              
        add (16|M0)              r16.0<1>:d    r12.0<8;8,1>:d    r8.6<0;1,0>:d    {Compacted}
        add (16|M0)              r24.0<1>:d    r12.0<8;8,1>:d    r8.7<0;1,0>:d    {Compacted}
        add (16|M16)             r18.0<1>:d    r14.0<8;8,1>:d    r8.6<0;1,0>:d   
        add (16|M16)             r26.0<1>:d    r14.0<8;8,1>:d    r8.7<0;1,0>:d   
        send (16|M0)             r20:w    r16     0xC            0x04205E00           // wr:2+0, rd:2; hdc.dc1; untyped surface read with x
        add (16|M0)              r32.0<1>:d    r12.0<8;8,1>:d    r9.0<0;1,0>:d    {Compacted}
        send (16|M0)             r28:w    r24     0xC            0x04205E01           // wr:2+0, rd:2; hdc.dc1; untyped surface read with x
        send (16|M16)            r22:w    r18     0xC            0x04205E00           // wr:2+0, rd:2; hdc.dc1; untyped surface read with x
        send (16|M16)            r30:w    r26     0xC            0x04205E01           // wr:2+0, rd:2; hdc.dc1; untyped surface read with x
        add (16|M16)             r34.0<1>:d    r14.0<8;8,1>:d    r9.0<0;1,0>:d   
        add (16|M0)              r20.0<1>:f    r20.0<8;8,1>:f    r28.0<8;8,1>:f   {Compacted}
        add (16|M16)             r22.0<1>:f    r22.0<8;8,1>:f    r30.0<8;8,1>:f  
        sends (16|M0)            null:w   r32     r20     0x8C            0x04025E02           // wr:2+2, rd:0; hdc.dc1; untyped surface write with x
        sends (16|M16)           null:w   r34     r22     0x8C            0x04025E02           // wr:2+2, rd:0; hdc.dc1; untyped surface write with x
(W)     send (8|M0)              null     r127    0x27            0x02000010           {EOT} // wr:1+0, rd:0; spawner; end of thread
L352:

```

## Runtime
The Compute Runtime https://github.com/intel/compute-runtime provides services for OpenCL and [Level Zero](https://github.com/oneapi-src/level-zero).  Level Zero  is the base interface for [oneApi](https://spec.oneapi.com/versions/latest/introduction.html).

It seems [SPIR-V](SPIRV.md) is used as the intermediate language.

Question: How do all the pieces that go into the Compute Runtime fit together?
