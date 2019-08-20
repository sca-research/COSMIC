# COSMIC: a Complication of Open-Source Masked Implementations for Crypto-software
Correctly implementing a side-channel countermeasure is never a trivial task. Although crypto-engineers often got the blame, frankly speaking, we do not always know for sure how to ``get it right" ourselves. For decades, writing the ``correct'' masking implementation on software platforms has been known as a ``time-consuming and error-prone'' task. Unlike on hardware platforms, we can hardly control the software compiling toolchain, which frequently nullifies our countermeasures with some not-so-smart register allocation or instruction ordering. We can, of course, avoiding the compiling effect by writing assembly ourselves; however, anything happens in the micro-architectural level or the memory hierarchy is still out of our hands. The ``order reduction theorem'' serves as a coarse-grained workaround: if the application requires $d$-order security, the implementation should implement a $2d$-order masking scheme\[BGGRS14\]. Considering the implementation cost grows exponentially with the security order, one can argue whether such security margin is indeed ``necessary". Besides, this theorem merely covers various bit-transitions: for any other type of leakage, there is no reason to believe the security reduction is still bounded by the factor 2.

We believe such ``engineering details'' are inevitable in side-channel analysis: to fully understand the threats one is facing, he or she has to test various concrete implementations among various software platforms, verifying whether there is any leakage left unprotected and what is the possible cause. Historically, this is indeed how side-channel analysis is discovered in the first place --- recognizing the potential misbelief between cryptographic theory and engineering facts.

Fortunately, more and more crypto-researchers tend to share their codes on Github in the last couple of years. We have collected some of these implementations and evaluated their security on our acquisition platforms with ARM Cortex M0/M3 cores. Thanks to Dr. Daniel Page's [SCALE project](https://github.com/danpage/scale), we now have various off-the-shelf target devices that provide easily accessible power measurement point. Of course, there are various other options serve for the same purpose: that has been said, this project is about analysing side-channel countermeasures (more specifically, "masking schemes") among practical implementation platforms. Although all tested results will be platform-specific, we are more interested in the potential cause of a certain countermeasure's failure, rather than the fact that it fails on a certain device. Revealing such implementation details helps us (as a cryptographic engineer) write better code for SCA protection, keep the protected implementation efficient and compact, even provide feedback to countermeasure designers about what cannot be assumed in realistic software platforms. To this end, we compiled a few open-source masked implementations on Github, together with our own version of scheme description and security evaluation. Please note that this repository is nothing more than a gallery: if you are considering using code from any submodule, please contact the original authors for permission.


## Module Summary

This is an on-going effort: more implementations will be added in the future.

|     Name (Author)      | Submodule Link | Programming Language | Target Platform | Protection Scheme | Protection Order  | 
| ---------------------- | -------------- | -------------------- |---------------- |  ---------------- |  ---------------- |
| ASM_MaskedAES (bristol-sca)  | https://github.com/bristol-sca/ASM_MaskedAES| Thumb16 | ARM M0 (or above) | Boolean masking (DPABook)| 1 |
| Byte-Masked-AES (Virginia Tech)  | https://github.com/gs1989/Masked-AES-Implementation/tree/master/Byte-Masked-AES| C | Portable | Boolean masking (DPABook)| 1 |
| bitsliced-masked-aes (Virginia Tech)  | https://github.com/gs1989/Masked-AES-Implementation/tree/master/bitsliced-masked-aes| C | Portable | 1-bit ISW multiplication| 1 |

<!-- ## Preformance \& Cost --> 
<!-- |     Name (Author)      | Randomness per block| Code size| Cycles/Executing time | RAM usage|  --> 

<!-- | ---------------------- | -------------- | -------------------- |---------------- |  ---------------- |  --> 
<!-- | ASM_MaskedAES (bristol-sca)  | 6B  | ??? | ??? | ??? |  --> 
<!-- | Byte-Masked-AES (Virginia Tech)  | 6B | ???  | ??? | ??? |  -->  
<!-- | bitsliced-masked-aes (Virginia Tech)  | 768B (pseudo random)| ???  | ??? | ??? |  --> 


## Share your work

We would appreciate if more authors are willing to share their work, on implementation code or security evaluation results. Please raise an issue or contact me at si.gao@bristol.ac.uk

## What we can provide
Currently we have,
- An ARM Cortex M0 (NXP LPC1114) board: 4K RAM
- An ARM Cortex M3 (NXP LPC1313) board: 8K RAM
- An atmega328p AVR microcontroller
- Other acquisition equipment: scopes, amplifiers, wave-generators, etc.

The following MIGHT be supported in the future,

- RISC-V processors
- More advanced cores, such as Cortex-M4

However, the following limitations exist (at least for right now),

- Do not support for specialised instruction set, such as ARM NEON
- RAM limitation: 4K or 8K, some already taken by the board support package
- GNU syntax for ARM assembly, no support for the ARM syntax

Non-specific T-test with leakage diagnosing can possibly be provided as a joint effort, yet we clearly do not have enough staff to cover every implementation. As said before, this part is indeed, painful and time-consuming. However, we do share our analysis/experience in this repository and hopefully other researchers will do so as well. Although these implementations themselves are probably not "academically interesting"; the causes why they fail in practice, definitely worth our attention as academic researchers.

## What we can expect
 
- Leakage detection: The non-specific property of the TVLA technique can be both bless and curse at the same time. On the bright side, TLVA does not depend on a specific power model or intermediate state, which in theory means it can possibly detect any leakage (up to a pre-determined order). On the other hand, it is also well known that such non-specific property makes it difficult to trace back the exact cause, or even confirm whether the reported sample is indeed a leakage. As a consequence, many assumptions we made in leakage detection are somehow left in the myth: for instance, we know for sure that one specific constant provides limited coverage. However, as ciphers usually involve iterations, we often assume if the selected constant cannot detect a certain flaw in round 1, there is a good chance that it will be reported in the following rounds. Unless we have enough tested implementations, there is no way to claim how well this assumption is respected in practice. Multi-variate adjustment is another example: we knew it is not fair to assume all samples on the trace are independent; however, how "biased" is this assumption has not been thoroughly tested, as we do not have an implementation where its leakage behavior is well-understood.

- Implementation guidelines: Implementation details raise a big concern for side-channel countermeasures in practice, yet we do not have any written guideline for crypto-engineers, reminding them what should be cautious about and how it should be done. Having a formal/systematic guideline could be difficult, but even ad-hoc notes could save precious time/effort for inexperienced developers. 

- Exemplar: Well-implemented codes for a certain type of countermeasure. Identifying the possible pitfalls and workarounds would definitely help the state-of-the-art.

- Feedback to the theoretical model: Many pitfalls may lie in the fact that our theoretical model where the countermeasure is designed and proved secure is incompatible with the device in practice. In case of any assumption is not trivial to ensure in practice, it might be worthwhile to reflect that back to the theoretical model, see if it is better to revise the model instead.

## References
\[BGGRS14\] Balasch, Josep, et al. "On the cost of lazy engineering for masked software implementations." International Conference on Smart Card Research and Advanced Applications. Springer, Cham, 2014.
