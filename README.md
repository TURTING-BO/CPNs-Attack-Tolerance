# CPNs-Attack-Tolerance
This is a case study of "A Coloured Petri Nets Based Attack Tolerance Framework". 

## Table of Contents

- [Background](#Background)
  - [Original Model](#original-model)
  - [Simulation Tool](#simulation-tool)
- [Case Study](#case-study)
  - [Original Modules](#original-modules)
  - [Attack Module](#attack-module)
  - [Tolerance Solution](#tolerance-solution)
- [References](#References)

## Background

### Original Model

The original model comes from reference [1], [2]. In the new model, a compound attack and tolerance solution is added. 

### Simulation Tool

When you want to simulate this example (i.e., [CaseStudy.cpn](https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/CaseStudy.cpn)), please make sure that you have installed CPNs Tools. CPNs Tools can be download from this website "http://cpntools.org/". There are many useful tutorials about how to use the powerful tool.

## Case Study

### Original Modules

In this case study, there are mainly four modules including _directory_, _patient_, _doctor_ and _cloud_. 

* The directory module receives query requests before responding to them. 
* The patient or doctor modules read or write records, where 
  - a patient can only read his/her own information;
  - a doctor can both read and write records of his/her patients. 
* The cloud module stores records of patients and manages reading and writing operations. 

A normal reading process of a patient is as follows.

1. When a patient needs to read records, he/she first sends a query request to the directory module. 
2. After receiving the request, the directory module responds to the patient with a provider list, indicating where his/her records are stored. 
3. Then, the patient constructs a read request according to the provider list and sends it to the cloud module. 
4. The cloud module supervises the reading process and returns the records if successful. 
5. Finally, the patient receives his/her records. 

*Note* that a patient's record is distributed in three providers to improve security. The read and write operations of doctors are similar to the read operation of patients. 

### Attack Module

Now consider three patients A, B, C and two doctors X, Y. The medical record of the patient C is ``{recID = "P3\_rec", data = "Name: Tom; Disease: COVID-19, Serious; Treatment: Critical;"}``. The doctor Y needs to read the record of C and determine the therapy. Consider an attack (see Doc_Y_read.pdf and Attack.pdf) aiming at making the doctorY read a wrong record ``{recID = "P3\_rec", data = "Name: Tom; Disease: Flu, Mild; Treatment: Basic;"}``, leading to incorrect diagnosis and treatment of the patient C, more seriously, losing C's life with no timely treatment!

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Module%20Figures/M1_Main.png"> 
  </div>
  <div align=center>
     <figcaption>Figure 1. Main Process</figcaption>
  </div>    
</figure>

This compound attack might consist in blocking (see Doc_Y_read.pdf), stealing and injection (see Attack.pdf). The attacker carries out the following steps:

1. First, he blocks the doctor Y to read the record of the patient C; 
2. Then, during the blocking time, he steals the record of C; 
3. Next, according to the stolen one, he modifies the record and injects it back to the O net; 
4. Finally, he cancels blocking, which allows the doctor Y to read an incorrect record.

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Module%20Figures/M6_Attack.png" width="70%" height="70%"> 
  </div>
  <div align=center>
     <figcaption>Figure 2. Attack</figcaption>
  </div>    
</figure>

In Figure 3, the attacker successfully modify C's record from ``Disease: COVID-19, Serious; Treatment: Critical;`` to ``Disease: Flu, Mild; Treatment: Basic;``.

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Result%20Figures/R2_Stealing_Modification_Injection%20Result.png" width="80%" height="80%"> 
  </div>
  <div align=center>
     <figcaption>Figure 3. Attack result</figcaption>
  </div>    
</figure>

### Tolerance Solution

Figure 4 and Figyre 5 present the bypassing solution and the checking solution to tolerate the above attack. 

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Module%20Figures/M5.2.1_Doc_Y_read.png"> 
  </div>
  <div align=center>
     <figcaption>Figure 4. Bypassing Solution</figcaption>
  </div>    
</figure>

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Module%20Figures/M3_Cloud.png"> 
  </div>
  <div align=center>
     <figcaption>Figure 5. Checking Solution</figcaption>
  </div>    
</figure>

Figure 6 and Figyre 7 present the simulation result of our solution to the attack, where the doctor Y can read the correct record of the patient C. 

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Result%20Figures/R4_After%20Checking%20Solution.png" width="80%" height="80%"> 
  </div>
  <div align=center>
     <figcaption>Figure 6. Checking and compensation result</figcaption>
  </div>    
</figure>

<figure>
  <div align=center>
    <img src="https://github.com/TURTING-BO/CPNs-Attack-Tolerance/blob/master/Result%20Figures/R6_Doctor%20Read%20Result.png" width="80%" height="80%"> 
  </div>
  <div align=center>
     <figcaption>Figure 7. Doctor Y reading result</figcaption>
  </div>    
</figure>

In Figure 6, our detector found this change and replaced the wrong record with the correct one, with the cooperation of bypassing solution shown in Figure 4. Finally, the doctor Y can read the correct record of C and decide a proper therapy.

The composition of bypassing, checking and compensation solutions can effectively tolerate the above attack (see Doc_Y_read.pdf and Attack.pdf).

## References

[1] Fitch D F, Xu H. A Petri Net Model for Secure and Fault-Tolerant Cloud-Based Information Storage[C]//SEKE. 2012: 333-339.

[2] Fitch D, Xu H. A RAID-based secure and fault-tolerant model for cloud information storage[J]. International Journal of Software Engineering and Knowledge Engineering, 2013, 23(05): 627-654.
