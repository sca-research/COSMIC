# COSMIC: a Complication of Open-Source Masked Implementations for Crypto-software


## Module Summary

|     Name (Author)      | Submodule Link | Programming Language | Target Platform | Protection Scheme | Protection Order  | 
| ---------------------- | -------------- | -------------------- |---------------- |  ---------------- |  ---------------- |
| ASM_MaskedAES (bristol-sca)  | https://github.com/bristol-sca/ASM_MaskedAES| Thumb16 | ARM M0 (or above) | Boolean masking (DPABook)| 1 |
| Byte-Masked-AES (Virginia Tech)  | https://github.com/gs1989/Masked-AES-Implementation/tree/master/Byte-Masked-AES| C | Portable | Boolean masking (DPABook)| 1 |
| bitsliced-masked-aes (Virginia Tech)  | https://github.com/gs1989/Masked-AES-Implementation/tree/master/bitsliced-masked-aes| C | Portable | 1-bit ISW multiplication| 1 |



[//]: <>( ## Preformance \& Cost)

[//]: <>( |     Name (Author)      | Randomness per block| Code size| Cycles/Executing time | RAM usage|)
[//]: <>( | ---------------------- | -------------- | -------------------- |---------------- |  ---------------- |)
[//]: <>( | ASM_MaskedAES (bristol-sca)  | 6B  | ??? | ??? | ??? |)
[//]: <>( | Byte-Masked-AES (Virginia Tech)  | 6B | ???  | ??? | ??? |) 
[//]: <>( | bitsliced-masked-aes (Virginia Tech)  | 768B (pseudo random)| ???  | ??? | ??? |) 