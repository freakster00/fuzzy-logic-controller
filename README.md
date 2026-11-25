

<!-- Start of picture text -->
‘fnga ‘ferafrererr<br>(ee.XOX4<br>i<br><!-- End of picture text -->

TRIBHUVAN UNIVERSITY INSTITUTE OF ENGINEERING PULCHOWK CAMPUS 

A PROPOSAL ON 

**DESIGN OF A GRID - AWARE FUZZY LOGIC CONTROLLER FOR ELECTRIC VEHICLE CHARGING IN DISTRIBUTION FEEDERS** 

SUBMITTED BY: 

NEERAJ BARI (PUL079BEL050) NIPURNA SHRESTHA (PUL079BEL052) ROHAN POUDEL (PUL079BEL074) SHLOK DAHAL (PUL079BEL085) 

SUBMITTED TO: 

DEPARTMENT OF ELECTRICAL ENGINEERING 

# **Acknowledgment** 

The authors would like to express their sincere gratitude to the Department of Electrical Engineering, Institute of Engineering, Pulchowk Campus, Tribhuvan University, for providing the opportunity, academic environment, and necessary resources to undertake this final-year project. 

We are deeply grateful to our respected project supervisor for their invaluable guidance, constructive suggestions, continuous encouragement, and technical support throughout the preparation of this project proposal. Their expertise and insightful feedback have been instrumental in shaping the direction of this work. 

We would also like to extend our appreciation to all the faculty members and staff of the Department of Electrical Engineering for their support, cooperation, and valuable knowledge throughout our undergraduate studies. 

Finally, we express our heartfelt gratitude to our parents, family members, and friends for their unwavering encouragement, patience, and moral support throughout the completion of this proposal. Their constant motivation has been a source of inspiration and strength in achieving this milestone. 

1 

# **Abstract** 

Integrating a large fleet of electric vehicles (EVs) into existing residential distribution systems without a dedicated communication infrastructure presents significant technical challenges for electric utilities. Unmanaged or “dump” charging can lead to severe power quality issues, including unacceptable voltage drops, transformer overloading, and increased feeder line losses, particularly at the downstream ends of radial feeders. This project proposes a robust, communication-free autonomous charging controller based on fuzzy logic to manage charging operations in real time without violating grid standard limits. 

The primary contribution of this work is the development of a grid-aware fuzzy inference system that prioritizes the health of the local distribution feeder. While most existing controllers rely on battery state of charge (SOC) or market price, this controller continuously monitors three critical feeder health indicators: bus voltage, distribution transformer loading, and real-time line losses. By utilizing “human-like” heuristic rules, the controller adaptively adjusts the EV charging rate to “shave” peak loads and ensure the grid operates within safe limits. 

Furthermore, the design incorporates a fair charging algorithm to prevent users at the end of the line from being unfairly penalized with slower charging rates due to their location. The proposed system is expected to be validated through high-fidelity MATLAB/Simulink simulations on a IEEE standard radial distribution network. Expected outcomes include significantly improved voltage regulation, reduced equipment thermal stress, and the ability to accommodate higher EV penetration levels compared to traditional uncoordinated or fixed-gain control methods. 

2 

# **List of Abbreviations** 

- **AC** Alternating Current **ANSI** American National Standards Institute **COG** Center of Gravity **CRF** Charging Rate Factor **DC** Direct Current **DoEE** Department of Electrical Engineering **EV** Electric Vehicle **FIS** Fuzzy Inference System **FLC** Fuzzy Logic Controller **IEEE** Institute of Electrical and Electronics Engineers **IOE** Institute of Engineering 

- **MATLAB** Matrix Laboratory 

**MV** Medium Voltage **NEA** Nepal Electricity Authority **PAR** Peak-to-Average Ratio **PCC** Point of Common Coupling **PDF** Probability Density Function **PEV** Electric Vehicle Charging Power **PU** Per Unit **RMS** Root Mean Square **SoC** State of Charge **THD** Total Harmonic Distortion **V2G** Vehicle-to-Grid 

3 

# **Contents** 

|**Ac**|**know**|**ledgment**|**1**|
|---|---|---|---|
|**Ab**|**strac**|**t**|**2**|
|**Lis**|**t of**|**Abbreviations**|**3**|
|**1**|**Intr**|**oduction**|**5**|
||1.1|Background . . . . . . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>5|
||1.2|Problem Statement<br>. . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>5|
||1.3|Objectives . . . . . . . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>6|
||1.4|Scope and Limitations<br>. . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>6|
|||1.4.1<br>Scope . . . . . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>6|
|||1.4.2<br>Limitations . . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>7|
|**2**|**Lite**|**rature Review**|**8**|
|**3**|**Met**|**hodology**|**9**|
||3.1|Overview . . . . . . . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>9|
||3.2|Problem Formulation . . . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>9|
||3.3|Fuzzy Inference System Design<br>. . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>11|
||3.4|Centroid Defuzzification . . . . . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>13|
||3.5|Flowchart of the Proposed Work . . . . . . . . . . . . . .|. . . . . . . . . . . . . .<br>15|
|**4**|**Exp**|**ected Outcomes**|**16**|



4 

# **Chapter 1** 

# **Introduction** 

## **1.1. Background** 

The rapid adoption of Electric Vehicles (EVs) is transforming modern power distribution systems by introducing new high-power residential loads. In Nepal, the increasing use of EVs is supported by the availability of clean hydroelectric energy and government initiatives aimed at reducing dependence on imported fossil fuels. While this transition offers significant environmental and economic benefits, it also places additional demand on existing low-voltage distribution networks. 

Most residential EV chargers operate in an uncontrolled manner, drawing their maximum rated power immediately after being connected to the grid. As EV penetration increases, simultaneous charging of multiple vehicles can lead to excessive voltage drop, transformer overloading, and increased feeder line losses, particularly during peak demand periods. Conventional solutions such as upgrading transformers and distribution feeders are effective but require significant investment and time. 

An alternative approach is intelligent EV charging, where charging power is adjusted according to the real-time operating condition of the distribution network. Among the available control techniques, fuzzy logic is particularly suitable because it can make effective control decisions using multiple grid parameters without requiring an accurate mathematical model of the system. This project therefore proposes a fuzzy logic-based, grid-aware EV charging controller that dynamically regulates charging power to improve distribution feeder performance under increasing EV penetration. 

## **1.2. Problem Statement** 

Uncontrolled EV charging does not consider the real-time operating condition of the distribution feeder supplying it. When an electric vehicle is connected to the grid, the charger typically begins operating at its maximum rated power regardless of the feeder’s existing voltage profile, transformer loading, or line loss condition. This results in a mismatch between the assumptions used during the design of the distribution network and the actual, uncoordinated charging demand experienced in real-time operation. 

As EV penetration increases, uncontrolled charging can lead to several technical challenges, including: 

- Bus voltage dropping below the permissible operating limits, thereby degrading the quality of power supplied to consumers. 

- Distribution transformer loading exceeding its rated capacity, accelerating thermal aging and increasing the likelihood of equipment failure. 

- Increased feeder line losses, resulting in reduced overall system efficiency and higher operational costs. 

5 

The fundamental issue is that conventional EV chargers operate without awareness of the electrical condition of the distribution network. Charging continues at full power until the battery is fully charged or disconnected, even when the feeder is operating close to its technical limits. Consequently, there is a need for an intelligent charging control mechanism capable of monitoring real-time grid conditions and dynamically regulating the EV charging rate to maintain feeder operation within acceptable voltage, loading, and loss limits. Such an approach can enhance distribution network reliability while delaying or avoiding costly infrastructure upgrades. 

## **1.3. Objectives** 

The primary objective of this project is to design and simulate a fuzzy logic-based, grid-aware EV charging control algorithm that protects the health of a distribution feeder under high EV penetration. The specific objectives are: 

1. To develop a representative low-voltage distribution feeder model using MATLAB/Simulink (Simscape Electrical) integrated with EV charging loads. 

2. To simulate and quantify the impact of uncontrolled EV charging on feeder performance by analyzing bus voltage profile, transformer loading, and feeder power losses under varying EV penetration levels. 

3. To design a fuzzy inference system (FIS) that utilizes real-time feeder parameters, namely bus voltage, transformer loading, and feeder losses, to determine an appropriate EV charging rate reduction factor. 

4. To integrate the proposed fuzzy logic controller into the distribution feeder model in a closed-loop configuration, enabling dynamic adjustment of EV charging power according to prevailing grid conditions. 

5. To evaluate and compare the performance of the proposed fuzzy logic-based charging strategy with conventional uncontrolled charging using voltage regulation, transformer loading, and feeder loss reduction as key performance indicators under multiple EV penetration scenarios. 

## **1.4. Scope and Limitations** 

### **1.4.1. Scope** 

This project focuses on the design, implementation, and simulation of a fuzzy logic-based, grid-aware EV charging controller for protecting the health of a low-voltage distribution feeder under high EV penetration. To ensure that the work remains feasible within the project duration, the scope is limited to the following: 

1. **Fuzzy Controller Inputs:** The proposed controller utilizes three real-time grid health indicators as input variables: 

   - Bus voltage (p.u.) at the EV connection point, 

   - Distribution transformer loading (%), 

   - Feeder line losses. 

2. **Control Output:** The controller produces a single _Charging Rate Factor_ (0–1), which proportionally regulates the EV charging current in real time. 

6 

3. **Simulation Environment:** The proposed system is modeled and evaluated using MATLAB/Simulink with Simscape Electrical. The simulation consists of a representative low-voltage residential distribution feeder comprising a single distribution transformer and multiple EV charging loads. 

4. **Fuzzy Inference System:** The controller employs a Mamdani-type fuzzy inference system using triangular and trapezoidal membership functions with the Center of Gravity (COG) defuzzification method. 

### **1.4.2. Limitations** 

The following limitations apply to the proposed study: 

1. **Limited Input Variables:** The fuzzy controller considers only three grid-health parameters: bus voltage, transformer loading, and feeder line losses. Other power quality indices such as Total Harmonic Distortion (THD), power factor, reactive power, and frequency deviation are not included. 

2. **Single-Phase Representation:** The distribution feeder is modeled using an aggregate singlephase representation. Consequently, three-phase unbalance and phase-specific loading effects are not investigated. 

3. **Grid-Oriented Control Only:** The controller is designed solely to protect distribution network performance. User-oriented factors such as electricity pricing, charging urgency, required stateof-charge (SoC), and charging preferences are not incorporated into the control strategy. 

4. **Fixed Rule Base:** The fuzzy inference system uses manually designed expert IF–THEN rules. No adaptive tuning, optimization techniques, or machine learning methods (e.g., ANFIS) are employed to optimize the rule base. 

5. **Single Feeder Configuration:** The investigation is limited to one representative low-voltage residential feeder. Therefore, the results may not directly generalize to feeders with different topologies, conductor characteristics, loading patterns, or network configurations. 

6. **Steady-State Analysis:** The study focuses on steady-state (RMS) feeder behavior and does not model electromagnetic transients, converter switching dynamics, or protection system interactions. 

7. **No Vehicle-to-Grid (V2G) Operation:** Only unidirectional EV charging is considered. Bidirectional power exchange between EVs and the grid is excluded from the present study. 

7 

# **Chapter 2** 

# **Literature Review** 

Several studies have investigated intelligent EV charging strategies to mitigate the adverse effects of high EV penetration on distribution networks. Among the various approaches, fuzzy logic has emerged as an effective technique due to its ability to process multiple uncertain inputs and make real-time charging decisions without requiring an accurate mathematical model of the power system. Hussain _et al._ [1] proposed a Hybrid Coordination Scheme (HCS) for residential EV charging using a fuzzy inference mechanism. The controller considers electricity price, required State-of-Charge (SoC), and household baseload as input variables to determine charging priority. Validated on the IEEE 34-bus distribution system, the proposed method reduced charging costs by up to 28%, improved user satisfaction by approximately 7%, and minimized the impact on the residential baseload. The study demonstrates the effectiveness of fuzzy logic in balancing grid performance and user charging requirements. 

Faddel, Mohamed, and Mohammed [2] developed a communication-free fuzzy logic controller that regulates EV charging using only local voltage measurements and battery SoC. Implemented in MATLAB/Simulink on an 18-bus distribution system, the controller maintained bus voltages within ANSI C84.1 limits and achieved fairer charging among EVs compared with conventional droop and exponential controllers. This work highlights the feasibility of intelligent charging using only locally available feeder information. 

Eajal _et al._ [3] presented a fuzzy logic-based charging controller employing minimum system voltage, battery SoC, and electricity price as input variables. The controller was tested on the IEEE 69bus distribution system and successfully maintained acceptable voltage levels while reducing feeder power losses compared with uncontrolled charging strategies. The authors also demonstrated an extension of the controller to Vehicle-to-Grid (V2G) operation, illustrating the flexibility of fuzzybased charging control. 

Kumar and Rao [4] proposed a constant-current fuzzy logic controller for grid-connected EV charging using MATLAB/Simulink. The controller regulates charging current according to battery SoC and available grid capacity, maintaining stable charging performance while preventing grid overload. Although the work focuses on a single charging unit rather than feeder-level analysis, it provides valuable guidance for charger modeling and parameter selection in simulation studies. 

The reviewed literature confirms that fuzzy logic is a mature and reliable approach for EV charging control. However, most existing controllers rely primarily on battery SoC, electricity price, or user preferences, while only limited attention has been given to simultaneously considering multiple feeder health indicators. This project addresses this gap by developing a grid-aware fuzzy logic controller that utilizes bus voltage, transformer loading, and feeder line losses to dynamically regulate EV charging, thereby enhancing distribution feeder reliability under high EV penetration. 

8 

# **Chapter 3 Methodology** 

## **3.1. Overview** 

This chapter presents the proposed methodology for developing a feeder-aware fuzzy logic controller (FLC) for coordinated electric vehicle (EV) charging in a residential distribution network. The controller continuously monitors the real-time operating condition of the feeder and dynamically regulates the EV charging rate to maintain acceptable network performance while satisfying user charging requirements. 

Unlike conventional uncontrolled charging, where every EV immediately charges at its rated power irrespective of feeder conditions, the proposed approach adapts the charging power according to the instantaneous health of the distribution system. The fuzzy inference system (FIS) evaluates four input variables representing both grid operating conditions and battery status, namely bus voltage, transformer loading, feeder line losses, and the battery State of Charge (SoC). Based on these inputs, the controller computes a normalized Charging Rate Factor (CRF), which proportionally adjusts the charging current supplied to each connected EV. 

The proposed methodology is inspired by the hybrid coordination framework presented by Hussain _et al._ [1]. However, instead of relying primarily on economic and behavioural variables such as electricity price, household baseload, and required SoC, the proposed controller directly reasons over electrical feeder-health parameters. Consequently, the charging decision is governed by the actual operating condition of the distribution feeder rather than external scheduling variables. 

## **3.2. Problem Formulation** 

The primary objective of the proposed controller is to maximize EV charging while ensuring that the operating limits of the distribution feeder are not violated. Rather than completely disconnecting EV chargers whenever the feeder approaches its limits, the controller continuously adjusts the charging rate according to the severity of feeder loading. Let 



represent the set of connected electric vehicles and 



represent the simulation time horizon. 

For each EV _h ∈H_ , the controller determines a normalized Charging Rate Factor 



9 

where 

### _CRFh_ ( _t_ ) = 1 

corresponds to full-rated charging and 



corresponds to complete charging suspension. 

Since maximizing the charging rate is equivalent to minimizing the charging reduction, the optimization objective may be expressed as 



subject to the technical operating constraints of the feeder. 

### **Voltage Constraint** 

To ensure acceptable power quality, the voltage magnitude at every monitored bus shall remain within the permissible operating limits specified by the distribution utility. 



following the commonly adopted voltage regulation limits for distribution systems. 

### **Transformer Loading Constraint** 

The apparent loading of the distribution transformer shall not exceed its rated thermal capacity. 



where 



denotes the percentage transformer loading. 

### **Feeder Line Loss Constraint** 

The feeder losses are evaluated using 



where 

- _Ik_ is the current flowing through feeder section _k_ , 

- _Rk_ is the resistance of feeder section _k_ , 

- _m_ denotes the total number of feeder sections. 

The controller attempts to maintain feeder losses below an acceptable operating threshold, 



thereby improving overall feeder efficiency. 

10 

### **Charging Rate Constraint** 

Since the controller proportionally regulates the charging current, the charging rate factor must satisfy 



for every connected EV. 

### **Fuzzy Input Vector** 

The controller continuously observes the feeder operating condition through the input vector 



where 

- _V_ bus denotes the bus voltage, 

- _L_ Tx denotes transformer loading, 

- _P_ loss denotes feeder line losses, 

- _SoCh_ represents the battery State of Charge of the _h_<sup>_th_</sup> EV. 

Unlike conventional charging strategies that treat every connected EV equally, the inclusion of battery SoC enables the controller to distinguish between urgent and non-urgent charging requirements. Consequently, EVs with a lower state of charge receive higher charging priority under identical feeder conditions, whereas EVs approaching full charge may be temporarily throttled to protect the distribution network. 

Accordingly, the fuzzy inference system may be represented as the nonlinear mapping 



where _F_ ( _·_ ) denotes the Mamdani fuzzy inference system developed in the subsequent section. The mathematical formulation presented in this section establishes the optimization objective and operational constraints that govern the proposed feeder-aware EV charging controller. The design of the fuzzy inference system used to realize this control strategy is described in the following section. 

## **3.3. Fuzzy Inference System Design** 

The proposed feeder-aware charging controller employs a Mamdani-type Fuzzy Inference System (FIS) to regulate the charging rate of connected electric vehicles according to the real-time operating condition of the distribution feeder. Fuzzy logic is selected because it effectively models nonlinear relationships between multiple operating variables without requiring an explicit mathematical model of the power system. 

The controller receives four input variables representing the instantaneous health of the feeder and the charging urgency of each EV, namely bus voltage, transformer loading, feeder line losses, and battery State of Charge (SoC). Based on expert-defined linguistic rules, the controller generates a normalized Charging Rate Factor (CRF) that proportionally adjusts the charging current supplied to each EV. 

11 

### **Input Variables** 

The fuzzy controller evaluates the following four input variables: 

1. Bus Voltage, _V_ bus (p.u.) 

2. Transformer Loading, _L_ Tx (%) 

3. Feeder Line Losses, _P_ loss (kW) 

4. Battery State of Charge, _SoC_ (%) 

These variables collectively represent the operating condition of the distribution feeder and the charging requirement of each EV. 

The controller output is the normalized Charging Rate Factor 



where 



### **Membership Function Design** 

Each input variable is converted into linguistic variables through the fuzzification process. Triangular and trapezoidal membership functions are employed because of their computational simplicity, smooth transition between operating regions, and widespread adoption in real-time power system control applications. 

The triangular membership function is defined as 



Similarly, the trapezoidal membership function is expressed as 



Triangular membership functions are adopted for intermediate operating regions because they provide higher sensitivity around the nominal operating point. Trapezoidal membership functions are employed at the extreme operating regions to provide wider regions of full membership, thereby improving controller robustness against measurement uncertainty. 

12 

### **Mamdani Inference Mechanism** 

The proposed controller employs the Mamdani fuzzy inference mechanism owing to its intuitive rule representation and widespread application in power system control. 

The firing strength of the _i_<sup>_th_</sup> rule is computed using the minimum operator 



where 



are the membership grades of the four input variables. 

The outputs of all activated rules are aggregated using the maximum operator 



where 



represents the aggregated output membership function. 

## **3.4. Centroid Defuzzification** 

The aggregated fuzzy output is converted into a crisp Charging Rate Factor using the Center of Gravity (Centroid) method. 

The centroid defuzzification is expressed as 



where 

- _y_ represents the output universe of discourse, 

- _µout_ ( _y_ ) is the aggregated output membership function, 

- Ω denotes the output domain. 

The centroid method produces a smooth and continuous control signal, thereby preventing abrupt variations in charging current that could adversely affect both the distribution feeder and the EV battery. Consequently, it is widely adopted in real-time fuzzy control applications and is employed in this study. 

Finally, the charging power of the _h_<sup>_th_</sup> EV is determined as 



where _Prated_ denotes the rated charging power of the EV charger. 

13 

Table 3.1: Summary of the Proposed Research Methodology 

|**Phase**|**Title**|**Main Output**|
|---|---|---|
|**Phase I**|Distribution Feeder Modeling and<br>Baseline Analysis|Development of the IEEE distribution feeder<br>model and evaluation of feeder performance un-<br>der uncontrolled EV charging.|
|**Phase II**|Development of the Fuzzy Logic<br>Charging Controller|Design and implementation of a Mamdani-type<br>fuzzy inference system with membership func-<br>tions and an expert-defined rule base for adap-<br>tive EV charging control.|
|**Phase III**|Controller Integration and Closed-<br>Loop Simulation|Integration of the fuzzy controller with the EV<br>charging system and simulation of closed-loop<br>charging under different EV penetration levels.|
|**Phase IV**|Performance Evaluation and Com-<br>parative Analysis|Comparison of controlled and uncontrolled<br>charging using feeder voltage profile, trans-<br>former loading, feeder line losses, and charging<br>performance metrics.|



14 

## **3.5. Flowchart of the Proposed Work** 



<!-- Start of picture text -->
START<br>Develop standard IEEE Feeder Model<br>(MATLAB/Simulink with Simscape Electrical)<br>Configure Distribution<br>Network Components<br>Distribution Transformer, Distribution Lines,<br>Residential Loads, EV Charging loads<br>Define EV Penetration Scenarios<br>(20%, 50%, 80%)<br>Perform Load Flow Simulation<br>under Uncontrolled EV Charging<br>Record Baseline Feeder Parameters<br>Bus Voltage, Transformer<br>Loading, Feeder Line Losses<br>Design Fuzzy Logic Controller<br>Define Fuzzy Input Variables<br>Bus Voltage, Transformer<br>Loading, Feeder Line Losses<br>Design Membership Functions<br>(Triangular/Trapezoidal)<br>Develop IF–THEN Fuzzy Rule Base<br>Apply Mamdani Inference<br>and COG Defuzzification<br>Generate Charging Rate Factor (CRF)<br>(0–1)<br>Integrate Fuzzy Controller into EV Charger<br>(Closed-Loop Charging Control)<br>Perform Controlled Charging Simulation<br>Measure Controlled Feeder Parameters<br>Bus Voltage, Transformer Loading,<br>Feeder Line Losses<br>Compare Controlled vs<br>Uncontrolled Charging<br>Analyze Performance Improvement<br>Draw Conclusions and Future Work<br>END<br><!-- End of picture text -->

15 

# **Chapter 4** 

# **Expected Outcomes** 

The proposed fuzzy logic-based, grid-aware EV charging controller is expected to improve the operational performance of low-voltage distribution feeders under increasing EV penetration. The anticipated outcomes of this project are as follows: 

1. **Improved Voltage Regulation:** The controller is expected to maintain bus voltages within acceptable operating limits by dynamically regulating EV charging power according to real-time feeder conditions. 

2. **Reduced Transformer Loading:** By limiting simultaneous high-power charging during peak demand periods, the controller is expected to prevent transformer overloading and improve asset reliability. 

3. **Lower Feeder Line Losses:** Adaptive charging control is anticipated to reduce feeder current during stressed operating conditions, thereby minimizing distribution line losses and improving overall system efficiency. 

4. **Robust and Adaptive Operation:** The fuzzy logic controller is expected to respond effectively to varying loading conditions using locally measured feeder parameters, ensuring reliable operation without requiring complex communication infrastructure. 

5. **Support for Future Smart Grids:** The developed control strategy is expected to provide a practical foundation for integrating intelligent EV charging into future smart distribution systems with higher renewable energy penetration. 

16 

# **Bibliography** 

- [1] M. Hussain, M. B. Rasheed, M. Awais, M. Aamir, _et al._ , “Hybrid Coordination Scheme Based on Fuzzy Inference Mechanism for Residential Charging of Electric Vehicles,” _Applied Energy_ , vol. 351, Art. no. 121963, 2023, Elsevier, doi: 10.1016/j.apenergy.2023.121963. 

- [2] S. Faddel, A. A. Mohamed, and O. A. Mohammed, “Fuzzy Logic-Based Autonomous Controller for Electric Vehicle Charging Under Different Conditions in Residential Distribution Systems,” _Applied Energy_ , vol. 183, pp. 1356–1367, 2016, Elsevier, doi: 10.1016/j.apenergy.2016.09.067. 

- [3] A. A. Eajal, M. F. Shaaban, E. F. El-Saadany, and K. Ponnambalam, “Fuzzy LogicBased Charging Strategy for Electric Vehicles Plugged into a Smart Grid,” in _Proc. IEEE Electrical Power and Energy Conference (EPEC)_ , Ottawa, ON, Canada, 2016, IEEE, doi: 10.1109/EPEC.2016.7771724. 

- [4] R. Kumar and K. V. S. Rao, “Constant Current Fuzzy Logic Controller for Grid Connected Electric Vehicle Charging,” _International Journal of Advanced Research in Science and Technology_ , vol. 12, no. 6, pp. 1–8, 2023. 

17 

