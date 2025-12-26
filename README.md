Sepsis-3 Revenue Integrity Predictor
📊 Project Overview & Business Impact
This project develops a predictive audit tool to identify Sepsis claims at risk of Clinical Validation Denial. In the US healthcare system, insurance payers often "downgrade" Sepsis claims if the clinical documentation does not meet the strict Sepsis-3 definition (life-threatening organ dysfunction), leading to significant revenue leakage.

Financial Risk Identified: Flagged $4,235,000 in potential revenue at risk within the test cohort.

Risk Prioritization: Identified 847 high-risk claims for pre-submission review by Clinical Documentation Improvement (CDI) specialists.

Final Model Performance: Achieved a 62% Recall for the high-risk class, ensuring the majority of vulnerable claims are captured for audit.

🛠️ Technical Evolution: From "Toy Model" to Reality
A critical part of this project was the iterative refinement of the machine learning pipeline to address common real-world pitfalls:

1. Identifying and Fixing Data Leakage

Initially, the model achieved a 1.00 ROC-AUC. This was identified as data leakage because the features used for prediction (Lactate and Creatinine) were the same variables used to define the "High Risk" label.

Correction: These "leakage" features were removed from the training set, resulting in a realistic and honest 0.63 ROC-AUC.

2. Solving the Recall Crisis

Without optimization, the initial realistic model had a Recall of only 0.03 (3%) for high-risk claims. In a billing environment, a model that misses 97% of the risk is unusable.

Correction: Implemented scale_pos_weight in the XGBoost classifier to handle severe class imbalance. This shifted the model's focus to the minority "High Risk" class, jumping Recall from 3% to 62%.

🧪 Data & Methodology
Dataset: Processed 67.5 GB of raw clinical data from MIMIC-IV 2.1 using memory-efficient Pandas chunking.

Cohort: Filtered 8,792 sepsis hospitalizations billed under DRG codes 870, 871, and 872.

Audit Logic: Claims were labeled "High Risk" if they were billed as Sepsis but lacked supporting organ failure evidence (Lactate < 2.0 and Creatinine < 1.2).

Feature Engineering: Developed predictors such as Length of Stay (LOS), Bilirubin levels, Platelet counts, and insurance types.

🚀 How to Use
Clone the repo: git clone https://github.com/Shubksiut/Sepsis-3.git

Relative Paths: The code is configured with relative paths (data/hosp). Ensure your MIMIC-IV data folder is placed in the project root.

Run the Analysis: Open Data Extraction.ipynb to view the full journey from data extraction to the final financial simulation.

📈 Top Predictors of Denial Risk
The final model shows that Length of Stay (los_days) is a primary driver of denial risk. This aligns with clinical reality: auditors are highly suspicious of intensive Sepsis billing for patients with very short hospital stays
