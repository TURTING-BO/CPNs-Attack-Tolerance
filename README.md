# CPNs-Attack-Tolerance
This is a case study of "A Coloured Petri Nets Based Attack Tolerance Framework".

The original model comes from reference [1], [2]. In the new model, a compound attack and tolerance solution is added. 

In this case study, there are mainly four modules including directory, patient, doctor and cloud. 
    The directory module receives query requests before responding to them. 
    The patient or doctor modules read or write records, where 
        a patient can only read his/her own information
        a doctor can both read and write records of his/her patients. 
    The cloud module stores records of patients and manages reading and writing operations. 
    
When a patient needs to read records, he/she first sends a query request to the directory module. After receiving the request, the directory module responds to the patient with a provider list,  indicating where his/her records are stored. Then, the patient constructs a read request according to the provider list and sends it to the cloud module. The cloud module supervises the reading process and returns the records if successful. Finally, the patient receives his/her records. Note that a patient's record is distributed in three providers to improve security. The read and write operations of doctors are similar to the read operation of patients. 

Now consider three patients $\mathit{A}$, $\mathit{B}$, $\mathit{C}$ and two doctors $\mathit{X}$, $\mathit{Y}$. The medical record of the patient $\mathit{C}$ is \{recID = ``P3\_rec", data = ``Name: Tom; Disease: COVID-19, Serious; Treatment: Critical;"\}. The doctor $\mathit{Y}$ needs to read the record of $\mathit{C}$ and determine the therapy. Consider an attack (Fig.~\ref{attack}) aiming at making the doctor $\mathit{Y}$ read a wrong record \{recID = ``P3\_rec", data = ``Name: Tom; Disease: Flu, Mild; Treatment: Basic;"\}, leading to incorrect diagnosis and treatment of the patient $\mathit{C}$, more seriously, losing $\mathit{C}$'s life with no timely treatment! This compound attack might consist in blocking (Fig.~\ref{docread}), stealing and injection (Fig.~\ref{cloud}). The attacker carries out the following steps: First, he blocks the doctor $\mathit{Y}$ to read the record of the patient $\mathit{C}$; then, during the blocking time, he steals the record of  $\mathit{C}$; next, according to the stolen one, he modifies the record and injects it back to the $\mathit{O}$ net; finally, he cancels blocking, which allows the doctor $\mathit{Y}$ to read an incorrect record.


Reference

[1] Fitch D F, Xu H. A Petri Net Model for Secure and Fault-Tolerant Cloud-Based Information Storage[C]//SEKE. 2012: 333-339.

[2] Fitch D, Xu H. A RAID-based secure and fault-tolerant model for cloud information storage[J]. International Journal of Software Engineering and Knowledge Engineering, 2013, 23(05): 627-654.
