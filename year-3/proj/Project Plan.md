# Dissertation Plan: Analysing the Effectiveness of Side-Channel Attacks on an Adaptive Encryption System Using Power Analysis


## Phase 1: Preparation and Background Research 
### 1. Abstract
A concise overview of the dissertation, outlining the research problem, objectives, methodology, and key findings. The aim will be to explore how effective power analysis side-channel attacks (specifically using the ChipWhisperer Lite) are against AES encryption, particularly when encryption parameters such as key size, encryption mode, and IV size are adapted for energy constraints.
### 2. Introduction
#### 2.1 Background and Motivation
- **Side-Channel Attacks (SCA):** Overview of side-channel attacks, with a focus on power analysis.
- **Adaptive Encryption Systems:** Why energy efficiency is crucial for devices such as IoT or mobile platforms.
- **Research Problem:** The trade-off between encryption strength and energy efficiency may introduce vulnerabilities. 
- **Motivation:** Investigating the vulnerabilities in AES encryption as parameters adjust, especially in power-constrained devices.
#### 2.2 Research Objectives
- Evaluate the effectiveness of power analysis attacks on AES with different key sizes, encryption modes, and IV sizes.
- Examine how variations in encryption parameters affect energy consumption and how these variations influence the susceptibility to side-channel attacks.
#### 2.3 Research Questions
- How do different AES key sizes, encryption modes, and IV sizes affect the vulnerability of an adaptive encryption system to power analysis attacks?
- Can side-channel attacks exploit energy-constrained devices adapting their encryption parameters?
- What is the relationship between energy consumption, encryption strength, and vulnerability?
- Sub-questions:
  - How do different AES modes affect attack effectiveness?
  - What is the impact of key and IV sizes on attack success rates?
  - How does the adaptive nature of the system influence its vulnerability?
### 3. Literature Review
#### 3.1 Overview of Encryption Systems
- Symmetric key encryption, with a focus on AES.
- Modes of operation in AES (e.g., CBC, ECB, GCM).
- Importance of initialisation vectors (IV) in encryption.
#### 3.2 Side-Channel Attacks (SCA)
- Overview of side-channel attacks, with a specific focus on power analysis (differential power analysis and simple power analysis).
- Historical examples of power analysis attacks on AES and similar encryption schemes.
- Impact of key size, mode of operation, and IV on side-channel vulnerabilities.
#### 3.3 Adaptive Encryption Systems
- Overview of systems that adapt encryption parameters (e.g., key size, encryption mode) to optimise for energy efficiency.
- Trade-offs between security and energy consumption in constrained devices (e.g., IoT, embedded systems).
#### 3.4 Tools and Platforms for SCA
- Overview of hardware platforms used in side-channel research.
- Detailed discussion of the ChipWhisperer Lite, its capabilities, and why it’s suitable for this study.
#### 3.5 Summary of Gaps in Existing Literature
- Lack of studies on adaptive encryption systems and their vulnerability to side-channel attacks when energy consumption is a factor.

## Phase 2: Experimental Design and Setup
### 4. Methodology

#### 4.1 Experimental Setup
- **ChipWhisperer Lite:** A low-cost hardware tool for conducting power analysis attacks.
- **Target Device:** Adaptive encryption system capable of adjusting encryption parameters (using AES).
- **Energy Measurement Tools:** Instrumentation to monitor power consumption during encryption.
#### 4.2 Encryption Parameters to be Tested
- **AES Key Sizes:** 128-bit, 192-bit, and 256-bit.
- **AES Modes of Operation:** ECB, CBC, GCM.
- **Initialisation Vector (IV) Sizes:** Standard vs. modified sizes, depending on implementation.
#### 4.3 Experimental Procedure
1. **Configure** the adaptive encryption system to adjust key size, encryption mode, and IV size based on predefined energy levels.
2. **Implement** the system on a power-constrained device (e.g., IoT, embedded device) to simulate real-world energy efficiency requirements.
3. **Execute** side-channel attacks (using the ChipWhisperer Lite) on each configuration:
    - Measure the system’s power consumption during encryption.
    - Capture power traces for analysis.
4. **Perform** the same set of power analysis attacks using differential and simple power analysis techniques across all configurations.
#### 4.4 Data Collection
- **Power Traces:** Collect power traces from the encryption process for each configuration.
- **Execution Time:** Record the encryption time under each configuration.
- **Energy Consumption:** Measure the energy consumption for different parameter settings.
- **Side-Channel Attack Success:** Measure the success rate of recovering the encryption key.

---

## 5. Data Analysis

### 5.1 Analyzing Power Traces
- Use signal processing techniques (e.g., filtering, noise reduction) to clean the power traces.
- Perform differential power analysis (DPA) and simple power analysis (SPA) to determine key leakage.

### 5.2 Evaluating Attack Effectiveness
- **Key Recovery Rate:** Compare the success rate of key recovery using DPA and SPA for each key size, encryption mode, and IV size.
- **Leakage Analysis:** Quantify the amount of information leaked during encryption as the key size and IV size change.
- **Impact of Mode:** Compare the vulnerability of different AES modes (e.g., CBC, GCM) to power analysis attacks.

### 5.3 Correlation Between Energy Consumption and Vulnerability
- Examine the relationship between the energy efficiency (adaptive encryption settings) and the system’s vulnerability to side-channel attacks.
- Identify trends in how lowering energy consumption increases susceptibility to power analysis attacks.

### 5.4 Statistical Analysis
- Perform statistical tests (e.g., ANOVA) to determine the significance of the differences in key recovery success rates across different configurations.

---

## 6. Discussion

### 6.1 Vulnerability of Adaptive Encryption Systems
- Discuss how key sizes, modes of operation, and IV sizes affect the effectiveness of side-channel attacks.
- Explore the trade-off between energy efficiency and security in adaptive encryption systems.

### 6.2 Implications for Energy-Constrained Devices
- Implications for the security of IoT devices, mobile devices, and other energy-constrained platforms using adaptive encryption systems.

### 6.3 Limitations and Challenges
- Discuss any limitations of the experimental setup (e.g., noise in power traces, hardware limitations).
- Consider how the results may differ with other side-channel attack techniques or more advanced hardware.

---

## 7. Conclusion

### 7.1 Summary of Key Findings
- Highlight the key insights gained from the experiments regarding AES vulnerability under different configurations.
- Emphasize the key contributions to understanding the impact of energy-adaptive encryption on security.

### 7.2 Recommendations for Future Research
- Suggest areas for further research, such as extending the study to other forms of adaptive encryption or exploring more advanced side-channel attack techniques.

### 7.3 Practical Implications
- Recommendations for designers of energy-efficient encryption systems to improve security while maintaining energy constraints.

---

## 8. References
A comprehensive list of all academic papers, books, and online resources cited in the dissertation.

---

## Appendices
- **Appendix A:** Raw Power Trace Data
- **Appendix B:** Statistical Analysis Results
- **Appendix C:** Experimental Setup Details (ChipWhisperer Lite configurations, system architecture)

---

## Final Notes

### Data Collection Tips
- Collect a significant number of power traces for each configuration (at least 1,000 traces per configuration) to improve the accuracy of the analysis.
- Ensure consistent environmental conditions during power trace collection to minimize noise.

### Tools
- **Python, MATLAB** or **Octave** for signal processing and statistical analysis.
- The ChipWhisperer’s own software for real-time analysis and visualization of side-channel attacks.

### Ethics and Security
Ensure that the research complies with ethical guidelines, especially if any part of the project involves handling or simulating sensitive data.



# Detailed Dissertation Plan: Analyzing Side-Channel Attacks on Adaptive AES Encryption

## Phase 1: Preparation and Background Research (Weeks 1-4)

### 1. Define your research question and objectives
- Primary question: "How effective are power analysis side-channel attacks against an energy-adaptive AES encryption system?"
- Sub-questions:
  - How do different AES modes affect attack effectiveness?
  - What is the impact of key and IV sizes on attack success rates?
  - How does the adaptive nature of the system influence its vulnerability?

### 2. Conduct a comprehensive literature review
- Search academic databases (IEEE Xplore, ACM Digital Library, Google Scholar)
- Topics to cover:
  - AES encryption (modes, key sizes, IV sizes)
  - Side-channel attacks, focusing on power analysis
  - Adaptive encryption systems and energy-efficient cryptography
  - ChipWhisperer platform and its applications

### 3. Develop a theoretical framework
- Identify key concepts and their relationships
- Create a conceptual model of your adaptive encryption system

### 4. Draft your introduction and literature review chapters

## Phase 2: Experimental Design and Setup (Weeks 5-8)

### 5. Design your adaptive encryption system
- Implement AES with adjustable parameters (mode, key size, IV size)
- Develop an energy measurement and adaptation mechanism

### 6. Set up the ChipWhisperer Lite
- Install necessary software and drivers
- Familiarize yourself with the ChipWhisperer API

### 7. Plan your experiments
- Define test cases:
  - AES modes: ECB, CBC, CTR
  - Key sizes: 128, 192, 256 bits
  - IV sizes: Standard and custom sizes
- Determine the number of encryption operations and power traces to collect for each configuration

### 8. Develop data collection scripts
- Write scripts to automate:
  - Encryption operations with varying parameters
  - Power trace collection
  - Energy consumption measurement
  - Timing measurements

### 9. Create a database schema for storing experimental results

## Phase 3: Data Collection (Weeks 9-12)

### 10. Conduct pilot tests
- Run a small-scale version of your experiments
- Verify data collection procedures and quality

### 11. Perform full-scale experiments
- Collect power traces for each AES configuration
- Measure energy consumption and encryption/decryption times
- Store all data in your prepared database

### 12. Implement power analysis attacks
- Develop scripts for Correlation Power Analysis (CPA)
- Attempt key recovery for each configuration
- Record success rates and number of traces required

### 13. Document all experimental procedures and observations

## Phase 4: Data Analysis (Weeks 13-16)

### 14. Preprocess and clean the collected data
- Handle any missing or anomalous data points
- Normalize data if necessary

### 15. Perform statistical analysis
- Calculate attack success rates for each configuration
- Analyze the relationship between energy efficiency and security
- Compare adaptive system performance to static configurations

### 16. Visualize your results
- Create graphs showing attack success rates vs. number of traces
- Plot energy consumption against security level
- Develop comparative charts for different AES modes and key sizes

### 17. Interpret your findings
- Identify patterns and trends in the data
- Relate results back to your research questions and hypotheses

## Phase 5: Writing and Revision (Weeks 17-20)

### 18. Write your methodology chapter
- Detail your experimental setup and procedures
- Explain your data collection and analysis methods

### 19. Draft your results chapter
- Present your findings clearly, using tables and figures
- Highlight key results that address your research questions

### 20. Develop your discussion chapter
- Interpret your results in the context of existing literature
- Discuss implications for adaptive encryption system design
- Address limitations of your study and suggest future research directions

### 21. Write your conclusion
- Summarize key findings
- Emphasize the significance and contributions of your work

### 22. Revise and refine all chapters
- Ensure logical flow and coherence throughout the dissertation
- Check for consistency in terminology and formatting

## Phase 6: Finalization and Presentation (Weeks 21-24)

### 23. Compile your full dissertation draft
- Assemble all chapters, including front matter (abstract, table of contents)
- Format according to university guidelines

### 24. Seek feedback
- Submit draft to your supervisor for review
- Consider peer review from classmates or colleagues

### 25. Make final revisions based on feedback

### 26. Prepare your defense presentation
- Create slides summarizing your research
- Practice your presentation

### 27. Submit your dissertation and defend your work

---

**Throughout the process:**
- Meet regularly with your supervisor for guidance
- Keep a detailed lab notebook of all your work
- Back up your data and drafts frequently
- Stay up-to-date with any new relevant publications in the field

Remember to adjust this timeline as needed based on your university's specific requirements and deadlines. Good luck with your research!