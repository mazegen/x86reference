# X86 Opcode and Instruction Reference

Work-in-progress repository. For more see [ref.x86asm.net](http://ref.x86asm.net).

### More on processors codes

The "introduced with processor" (XML `proc_start`) really made sense only up to some of the early Pentium processors. However, the XML reference codes until `13` (Core i7) are not going to be reconsidered because of backwards compatibility. For future, it is proposed to use Intel processor families instead:

| Intel CPU Family    | HTML editions code | XML reference code | Introduced | New instructions, ISA additions | Notes |
|---------------------|------|----|------|------------------|---------------------|
| Nehalem, Bloomfield | `C7` | 13 | 2008 | SSE4.2, `POPCNT` | already implemented
| Westmere, Gulftown  | `WM` | 14 | 2010 | AES-NI, `PCLMULQDQ` |
| Sandy Bridge        | `SB` | 15 | 2011 | AVX |
| Ivy Bridge          | `IB` | 16 | 2012 | `RDRAND`, F16C, FSGSBASE |
| Haswell             | `HW` | 17 | 2013 | AVX2, FMA3, BMI1/BMI2, TSX (HLE/RTM) |
| Broadwell-Y         | `BW` | 18 | 2014 | ADX (`ADCX`/`ADOX`), `RDSEED`, `PREFETCHW`, SMAP |
| Skylake             | `SL` | 19 | 2015 | SGX, MPX, XSAVEC/XSAVES, CLFLUSHOPT |
| Kaby Lake           | `KL` | 20 | 2016 | PTWRITE |
| Knights Landing     | `KN` | 21 | 2016 | AVX-512 (first shipping implementation) |
| Cannon Lake         | `CL` | 22 | 2018 | AVX-512 IFMA, AVX-512 VBMI |
| Cascade Lake-SP     | `CA` | 23 | 2019 | AVX-512 VNNI |
| Ice Lake            | `IL` | 24 | 2019 | GFNI, VAES, VPCLMULQDQ, AVX-512 VNNI; other AVX-512 add-ons like VBMI2, BITALG, VPOPCNTDQ |
| Tremont             | `TR` | 25 | 2020 | WAITPKG (UMONITOR/UMWAIT/TPAUSE) |
| Alder Lake          | `AL` | 26 | 2021 | AVX-VNNI, new instructions like `SERIALIZE`, `HRESET` |

### Notes on Addressing Methods

#### The `J` method

Might be confusing. For example, `Jbs` actually reads as: "Relative offset to be added to the IP register. The relative offset is sign-extended to the size of of the IP register."

This method is always used as a source operand. To make it completely correct, it should be a destination operand but it would make the byte value also the destination and that doesn't make much sense. This would need to be solved by introducing new types of operands but I don't think it's worth it.

### Notes on Operand Type codes

#### SIMD FP instructions with integer codes

There are several MOV-like SIMD instructions that operate on floating-point data but their operands are indicated as integer ones. For example [`MOVHLPS Vq, Uq`](http://ref.x86asm.net/geek.html#x0F12) means "Move two packed single precision floating-point values from high quadword of source XMM register to low quadword of destination XMM register", however, all the Intel manuals from the pre-AVX era doesn't indicate its operands as packed single FP values (`ps` code) in the opcode map. This is presumably because the operands are treated as integers during the move operation.

The newer manuals from the AVX era indicate this instruction as `VMOVHLPS Vq, Hq, Uq` in the opcode map, making it unclear how the non-AVX version should look like. Anyway, even the AVX version uses integer codes.
