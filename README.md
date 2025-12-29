Project Context:
Autism lacks definitive diagnostics, so this project predicts ASD risk from behavioral questionnaire data (A1-A10 scores), demographics (age, gender, ethnicity), and clinical features using a public dataset of 800 samples. The goal was early detection via ML classification of 'Class/ASD' (YES/NO) .

Simple Data Cleaning
Loaded the CSV file (like opening an Excel sheet) using pandas library.
Fixed messy entries: Changed "?" or "others" to "Others" for consistency.
Turned "yes/no" answers into numbers (1 for yes, 0 for no) so computer can read them.
Removed 2 weird outlier rows from "result" column, leaving 798 clean rows with mix of numbers and categories .

Easy EDA (Data Exploration):
Made pie chart: Showed fewer people have ASD (imbalanced data—like more healthy than sick samples).
Count charts: High scores in A1-A10 questions (social behaviors) link strongly to ASD.
Saw age is skewed (mostly young kids), so used log math to normalize it.
Created "sum_score" by adding up A1-A10 scores—this became the stron

Straightforward Feature Making
Grouped ages into buckets: "Toddler", "Kid", etc. (instead of exact numbers).
Added "sum_score" (total of 10 behavior questions) and "ind" (sum of autism family history + jaundice + app use).
Changed text labels to numbers; dropped useless columns like "age_desc" (too similar to age) and "used_app_before" (too linked to others) .

Basic Model Steps
Split data: 80% for training, 20% for testing.
Fixed imbalance: RandomOverSampler copied minority ASD samples to match majority.
Scaled numbers: StandardScaler made all features same size range.
Tried 3 models: LogisticRegression (simple lines), XGBoost (tree power), SVC (curvy boundaries)—SVC and Logistic got best score (~98% ROC-AUC, like 98% correct predictions) without overfitting. Built a function to predict on new patient data .


Flow:

┌─────────────────┐
│ 1. Load Data    │ ← train.csv (800 rows)
│    (pandas)     │
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ 2. Clean Data   │ ← Fix ?/others → "Others"
│    - yes/no → 1/0│   Drop 2 outliers → 798 rows
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ 3. Explore (EDA)│ ← Pie: Imbalance (ASD minority)
│    - Countplots │   A-scores ↑ → ASD ↑
│    - sum_score  │   Log age, country effects
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ 4. New Features │ ← ageGroup (Toddler/Kid)
│    - sum_score  │   ind (austim+jaundice+app)
│    - Drop extras│
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ 5. Prepare Data │ ← 80/20 split
│    - Oversample │   Scale (StandardScaler)
│    (RandomOver) │
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ 6. Train Models │ ← Logistic, XGBoost, SVC
│    Best: SVC    │   98% ROC-AUC (no overfit)
│    + Predict fn │
└─────────────────┘


