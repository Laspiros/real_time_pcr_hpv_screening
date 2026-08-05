🧬 Real-Time PCR HPV Screening: Clinical Triage & Lab Quality Analytics
An end-to-end clinical data science and bio-operational pipeline applied to raw genomic laboratory exports. This repository transforms unstructured optical outputs from an Agilent AriaDx Real-Time PCR instrument into actionable epidemiological insights, automated patient triage pathways, and physical laboratory hardware diagnostics. 
📌 Executive Summary
Raw diagnostic assay outputs often blend physical optical metrics with clinical endpoints. This project demonstrates a end-to-end data pipeline processing 36,096 raw rows across 96 PCR runs (12 batch files): 
	Data Engineering & Feature Pruning: Reduced 35 raw optical/calibration columns down to 6 core clinical and metadata variables. 
	Biological & Target Disambiguation: Identified target roles without manufacturer documentation, isolating the CY5 Internal Control from high-risk HPV clinical targets (FAM, ROX, HEX). 
	Clinical Decision Support (CDS): Built an automated triage engine stratifying patient risk (Colposcopy vs. Repeat vs. Routine). 
	Non-Parametric Statistical Testing: Proved via Mann-Whitney U test (p=0.7987) that infection complexity (co-infections) does not correlate with significantly higher viral loads. 
	Operational Quality Auditing: Diagnosed a ~29.65% global "invalid rate" as an operational artifact resulting from empty/NTC wells in Column 12 of 96-well plates. 

📂 Dataset Overview & Privacy Compliance
	Data Source: Agilent AriaDx Real-Time PCR qualitative/semi-quantitative screening assay exports. 
	Dimensions: 36,096 total rows across 9,216 processed wells (12 batches ×8 runs ×96 wells). 
	Privacy & Anonymization: Stripped of all Protected Health Information (PHI) to comply with GDPR/HIPAA. Samples are tracked exclusively via anonymized batch identifiers and spatial well coordinates. 

🔬 Key Analysis Phases & Findings
Phase 1 & 2: Ingestion, Pruning & Feature Selection
The raw export contains 35 attributes per target record. Unused background math (R Last, ∆R Last, Threshold), melt curve variables, and unpopulated metadata columns were pruned. 

Phase 3: Target Identification & Viral Load Profiling
Through positivity distributions and Cq dispersion analysis, channel identities were disambiguated: 
	CY5 (Internal Extraction Control): Amplified in 6,416 wells with a tight, highly predictable distribution (median Cq ~16.2), confirming extraction efficiency. 
	FAM, ROX, HEX (High-Risk HPV Targets): Exhibited lower positivity rates (200–349 hits each) and wide Cq distributions (cycles 18–32), capturing natural biological variations in viral load. 
Viral Load Stratification
	High Load ("Cq"<20): Early crossings indicating severe, high-titer infections. 
	Moderate Load (20≤"Cq"≤30): Standard baseline for active clinical infections. 
	Low Load ("Cq">30): Borderline detections near the assay's limit of detection (LoD). 

Phase 4: Patient Co-infection Profiling
Filtering out empty wells yielded 6,483 valid patient samples: 
	Negative (0 Hits): 5,780 samples (89.2%) 
	Single Infection: 610 samples 
	Biological Co-infection (2 Hits): 68 samples 
	Positive Controls (3 Hits): 25 samples (synthetic positive calibrators) 

Phase 5: Clinical Decision Support (CDS) Rules Engine
Raw target responses were mapped to clinical risk pathways: 
	High Risk (Immediate Colposcopy Referral): Positive for FAM (HPV 16) or ROX (HPV 18). →577 Patients 
	Moderate Risk (Repeat Screen in 1 Year): Positive for HEX (Other HR-HPV) only. →126 Patients 
	Routine Screening (Negative): Negative for HPV targets with valid CY5 internal control. →5,780 Patients 

Statistical Analysis: Infection Complexity vs. Viral Load
We tested whether carrying multiple high-risk genotypes (co-infections) drives higher viral replication rates than single infections. 
	Methodology: Non-parametric Mann-Whitney U Test performed on Cq values (excluding 3-hit positive controls).
	Single Infection Median Cq: 25.89
	Co-infection Median Cq: 25.12
	Result: p=0.7987(p>0.05)
	Conclusion: Fail to reject the null hypothesis. Infection complexity does not significantly alter median viral loads in this population. 

Phase 6: Operational Audit & The "Column 12" Artifact
Initial evaluation revealed a 29.65% Global Invalid Rate (2,733 wells). Spatial coordinate mapping resolved the cause: 
	Top invalid well positions: C12, E12, F12, H12, and G12. 
	Insight: In standard batching protocols, unassigned wells on 96-well plates are left empty or filled with No Template Controls (NTCs). The assay correctly flagged these as invalid (lacking CY5 and viral targets). 
	Strategic Value: Demonstrates the importance of dual-dataset management—filtering invalid wells for clinical epidemiology while preserving them for lab workflow audits. 

🚀 Running the Notebook
Execute the full analytical notebook via Jupyter or Kaggle

📜 License & Citation
Distributed under the Creative Commons Attribution 4.0 International (CC BY 4.0) License. You are free to share and adapt this material for academic or commercial purposes with attribution.

