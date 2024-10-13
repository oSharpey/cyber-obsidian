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
Define test cases: a. AES modes: ECB, CBC, CTR b. Key sizes: 128, 192, 256 bits c. IV sizes: Standard and custom sizes
Determine the number of encryption operations and power traces to collect for each configuration
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
## Phase 3: Data Collection
### 5 Data Collection
- **Power Traces:** Collect power traces from the encryption process for each configuration.
- **Execution Time:** Record the encryption time under each configuration.
- **Energy Consumption:** Measure the energy consumption for different parameter settings.
- **Side-Channel Attack Success:** Measure the success rate of recovering the encryption key.
#### 5.1 Conduct pilot tests
- Run a small-scale version of your experiments
- Verify data collection procedures and quality

#### 5.2 Perform full-scale experiments
- Collect power traces for each AES configuration
- Measure energy consumption and encryption/decryption times
- Store all data in your prepared database

#### 5.3 Implement power analysis attacks
- Develop scripts for Correlation Power Analysis (CPA)
- Attempt key recovery for each configuration
- Record success rates and number of traces required

## Phase 4: Data Analysis
### 6. Data Analysis
#### 6.1 Analysing Power Traces
- Use signal processing techniques (e.g., filtering, noise reduction) to clean the power traces.
- Perform differential power analysis (DPA) and simple power analysis (SPA) to determine key leakage.
#### 6.2 Evaluating Attack Effectiveness
- **Key Recovery Rate:** Compare the success rate of key recovery using DPA and SPA for each key size, encryption mode, and IV size.
- **Leakage Analysis:** Quantify the amount of information leaked during encryption as the key size and IV size change.
- **Impact of Mode:** Compare the vulnerability of different AES modes (e.g., CBC, GCM) to power analysis attacks.
#### 6.3 Correlation Between Energy Consumption and Vulnerability
- Examine the relationship between the energy efficiency (adaptive encryption settings) and the system’s vulnerability to side-channel attacks.
- Identify trends in how lowering energy consumption increases susceptibility to power analysis attacks.
#### 6.4 Statistical Analysis
- Perform statistical tests (e.g., ANOVA) to determine the significance of the differences in key recovery success rates across different configurations.
- Calculate attack success rates for each configuration
- Analyse the relationship between energy efficiency and security
- Compare adaptive system performance to static configurations
#### 6.5 Visualise results
- Create graphs showing attack success rates vs. number of traces
- Plot energy consumption against security level
- Develop comparative charts for different AES modes and key sizes
## Phase 5: Discussion and Conclusion
### 7. Discussion
#### 7.1 Vulnerability of Adaptive Encryption Systems
- Discuss how key sizes, modes of operation, and IV sizes affect the effectiveness of side-channel attacks.
- Explore the trade-off between energy efficiency and security in adaptive encryption systems.
- **Key Size**: Discuss the differences in vulnerability to power analysis attacks across different AES key sizes (128-bit, 192-bit, and 256-bit). Did the larger key sizes provide better protection, as theory suggests? Did the power analysis attacks succeed more easily with smaller keys?
	- For example, if **AES-128** was significantly more vulnerable to differential power analysis (DPA) than AES-256, you can speculate that the reduced number of key rounds may provide more opportunities for attackers to exploit leakage from intermediate states.
	- Consider also any **trade-offs** between the smaller key sizes and lower energy consumption—was AES-128 more vulnerable, but also more energy-efficient? How should that trade-off be interpreted?
- **Energy-Adaptive Encryption**: Focuses is the adaptability of encryption to energy constraints. How did the system’s ability to **dynamically adjust parameters** based on energy needs impact its vulnerability to side-channel attacks? Did the encryption system become significantly more vulnerable when trying to conserve energy?
    - If the system shifted to smaller key sizes or simpler modes to save power, discuss how these adjustments correlate with a **higher rate of key recovery**. Highlight if there's a particular **tipping point** where energy savings severely compromised security (e.g., when key size drops below 192 bits).
- **Correlation Between Energy and Attack Success**: Based on statistical findings (such as correlation and regression analysis), discuss the relationship between **power consumption** and attack success rates. Was there a clear positive correlation, indicating that as energy consumption decreased, the system became more vulnerable?
    - For instance, if you found that **lower power settings** led to **higher leakage**, explore the implications for real-world systems like IoT devices, where energy efficiency is critical. Can this result be generalised across similar adaptive encryption systems?
#### 7.2 Implications for Energy-Constrained Devices
- Implications for the security of IoT devices, mobile devices, and other energy-constrained platforms using adaptive encryption systems.
#### 7.3 Limitations and Challenges
- Discuss any limitations of the experimental setup (e.g., noise in power traces, hardware limitations).
- Consider how the results may differ with other side-channel attack techniques or more advanced hardware.
### 8. Conclusion
#### 8.1 Summary of Key Findings
- Highlight the key insights gained from the experiments regarding AES vulnerability under different configurations.
- Emphasize the key contributions to understanding the impact of energy-adaptive encryption on security.
#### 8.2 Recommendations for Future Research
- Suggest areas for further research, such as extending the study to other forms of adaptive encryption or exploring more advanced side-channel attack techniques.
#### 8.3 Practical Implications
- Recommendations for designers of energy-efficient encryption systems to improve security while maintaining energy constraints.

## Appendices
- **Appendix A:** Raw Power Trace Data
- **Appendix B:** Statistical Analysis Results
- **Appendix C:** Experimental Setup Details (ChipWhisperer Lite configurations, system architecture)

## Final Notes

### Data Collection Tips
- Collect a significant number of power traces for each configuration (at least 1,000 traces per configuration) to improve the accuracy of the analysis.
- Ensure consistent environmental conditions during power trace collection to minimise noise.
### Tools
- **Python, MATLAB** or **Octave** for signal processing and statistical analysis.
- The ChipWhisperer’s own software for real-time analysis and visualisation of side-channel attacks.
### Ethics and Security
Ensure that the research complies with ethical guidelines, especially if any part of the project involves handling or simulating sensitive data.
