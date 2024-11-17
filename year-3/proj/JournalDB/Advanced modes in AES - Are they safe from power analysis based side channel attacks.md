---
Status: Not started
tags:
  - AES
  - Experimental
  - Power-analysis
DOI: 10.1109/ICCD.2014.6974678
CreatedTime: 2024-10-12T16:52
---
```Markdown
Write down the structure of the research paper.
```

> [!important]  
> \<authors\> explore the security of advanced AES modes against CPA attacks, namely Cipher Block Chaining (CBC), Cipher Feedback (CFB), Output Feedback (OFB), and Counter (CTR). The authors focus their attacks on low-power embedded systems, specifically the SASEBO-GII platform - a side-channel analysis platform similar to the ChipWhisperer. The authors show that these advanced modes remain vulnerable to CPA attacks. Specifically, the ECB mode can be broken with around 2016 traces, whereas CBC, CFB, OFB, and CTR require progressively more traces to obtain the key. The authors cover their methodology in depth for each mode, using their power trace results and analysis to recommend when keys are changed - they do not mention, however, any further mitigations for these attacks, like noise injection. This leaves space for the further analysis of these attacks with advanced mitigations in place.

[]

---

- Paper look at the effectiveness of side channel attacks against modern modes of aes
