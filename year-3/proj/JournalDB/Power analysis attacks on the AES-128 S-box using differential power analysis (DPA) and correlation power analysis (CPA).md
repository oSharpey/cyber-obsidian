---
Status: Not started
tags:
  - AES
  - CPA
  - DPA
  - Experimental
DOI: 10.1080/23742917.2016.1231523
CreatedTime: 2024-10-12T16:52
---
```Markdown
Write down the structure of the research paper.
```

> [!important]  
> Owen Lo, William J. Buchanan, and Douglas Carson examine DPA and CPA attacks on the Arduino Uno, measuring which attack is more effective. The authors focus on two primary steps in AES-128: AddRoundKey and SubBytes. The authors show that both attacks were capable of retrieving the entire 16-byte key however, the results from CPA are found to be easier to interpret analytically due to its reduced noise in the traces compared to DPA. The authors acknowledge the limitations of their white-box method, noting that these attacks may be more difficult on black-box implementations of AES-128. The author’s in-depth explanation of these attacks and pseudocode provide a strong foundation for the basis of this project. However, a common theme in power analysis studies, like \<authors\> work,  is the lack of research on larger key sizes of AES like 192 or 256 which could be utilised in an adaptive encryption system.

[](https://www.notion.soundefined)

---

- Note down the key points.