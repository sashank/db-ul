# Exercise 8.4B: One-Class SVM for Database Access Anomaly Detection

## 📚 Complete Step-by-Step Implementation

**Duration**: 3-4 hours  
**Difficulty**: Intermediate-Advanced  
**Status**: Part 1 Complete ✅

---

## 🎯 Exercise Overview

This exercise teaches you how to use **One-Class SVM** to detect anomalous database access patterns that could indicate:
- Compromised administrator credentials
- Insider threat activity  
- Reconnaissance attacks
- Mass data exfiltration

---

## 📂 Project Structure

```
Exercise_8_4B_Database_Anomaly_Detection/
├── Part1_Data_Generation.ipynb          ✅ COMPLETE
├── Part2_Feature_Engineering.ipynb      ⏳ NEXT
├── Part3_OneClass_SVM_Training.ipynb    📋 TODO
├── Part4_Attack_Detection.ipynb         📋 TODO
├── Part5_Operational_Deployment.ipynb   📋 TODO
├── data/
│   ├── baseline_logs.csv
│   ├── attack_logs.csv
│   ├── complete_logs.csv
│   └── summary_stats.json
├── models/
│   └── (trained One-Class SVM models will be saved here)
└── results/
    └── (visualizations and reports will be saved here)
```

---

## ✅ Part 1: Data Generation (COMPLETE)

**What You Built:**
- Realistic synthetic database access logs
- 28 days of normal baseline data (3 administrators)
- 3 attack scenarios injected

**Administrator Profiles:**
1. **admin_alice** (Data Analyst)
   - Frequent SELECT queries
   - Business hours: 8 AM - 6 PM
   - Tables: customers, transactions, orders

2. **admin_bob** (System Administrator)
   - Mixed operations (SELECT, UPDATE, INSERT, DELETE)
   - Extended hours: 7 AM - 10 PM
   - Tables: audit_logs, config, admin tables

3. **admin_charlie** (Developer)
   - Complex JOINs for testing
   - Flexible hours: 9 AM - 8 PM
   - Tables: products, orders, customers

**Attack Scenarios Injected:**
1. **Reconnaissance**: Attacker systematically queries all tables with SELECT *
2. **Mass Extraction**: 10-20× normal row counts from sensitive tables
3. **Credential Theft**: Unusual time (3 AM) + atypical table access

**Key Files Generated:**
- `data/baseline_logs.csv` - Training data (normal behavior only)
- `data/attack_logs.csv` - Validation data (attack scenarios)
- `data/complete_logs.csv` - Full dataset

---

## ⏳ Part 2: Feature Engineering (NEXT STEP)

**What You'll Build:**

### Phase 1: Per-Query Features (30 min)
Extract features from each database query:
- Query complexity metrics
- Table access patterns
- Temporal features
- Resource consumption

### Phase 2: Aggregated Behavioral Features (30 min)
Aggregate per-admin over time windows (daily):
- Query type distribution
- Table access frequency
- Timing patterns
- Anomaly indicators

### Phase 3: Feature Normalization (15 min)
Prepare features for One-Class SVM:
- StandardScaler for consistent scales
- Handle missing values
- Create training/test splits

**Expected Output:**
- `data/features_alice.csv`
- `data/features_bob.csv`
- `data/features_charlie.csv`
- Feature correlation matrix
- Feature distribution plots

---

## 📋 Part 3: One-Class SVM Training (TODO)

**What You'll Build:**

### Phase 1: Baseline Model Training (30 min)
- Train One-Class SVM per administrator on normal data
- Try both Linear and RBF kernels
- Tune `nu` parameter (expected anomaly fraction)

### Phase 2: Model Evaluation (30 min)
- Analyze support vectors
- Validate on held-out normal data
- Check false positive rate on known-good queries

**Expected Output:**
- `models/ocsvm_alice.pkl`
- `models/ocsvm_bob.pkl`
- `models/ocsvm_charlie.pkl`
- Training metrics report

---

## 📋 Part 4: Attack Detection (TODO)

**What You'll Build:**

### Phase 1: Score Attack Scenarios (30 min)
- Score attack logs with trained models
- Analyze anomaly scores
- Compare to normal baseline scores

### Phase 2: Performance Evaluation (30 min)
- Compute TPR, FPR, Precision, Recall
- ROC curves per admin
- Confusion matrices

### Phase 3: False Positive Analysis (30 min)
- Investigate false positives
- Understand model limitations
- Document edge cases

**Expected Output:**
- Detection performance report
- ROC/PR curves
- False positive analysis document

---

## 📋 Part 5: Operational Deployment (TODO)

**What You'll Build:**

### Phase 1: Alert Workflow Design (30 min)
- High/Medium/Low confidence tiers
- Automated response actions
- Analyst investigation procedures

### Phase 2: Monitoring Dashboard (30 min)
- Real-time anomaly feed
- Alert prioritization
- Historical trend analysis

### Phase 3: Model Lifecycle (15 min)
- Retraining procedures
- Drift detection
- Feedback incorporation

**Expected Output:**
- Operational playbook
- Alert workflow diagram
- Deployment architecture

---

## 🚀 How to Use This Exercise

### For Students:

**Beginner Path** (4 hours):
1. Run Part 1 to understand data generation
2. Study the attack patterns carefully
3. Complete Parts 2-3 with full explanations
4. Skim Parts 4-5, understand key concepts

**Intermediate Path** (3 hours):
1. Quickly review Part 1 data
2. Complete Parts 2-4 thoroughly
3. Focus on parameter tuning in Part 3
4. Implement custom attack scenarios

**Advanced Path** (2.5 hours):
1. Generate your own attack variations
2. Experiment with different feature sets
3. Compare Linear vs RBF kernels systematically
4. Build production-ready deployment plan

### Running the Notebooks:

```bash
# Start Jupyter
jupyter notebook

# Navigate to Exercise_8_4B_Database_Anomaly_Detection/
# Open Part1_Data_Generation.ipynb
# Run all cells sequentially
# Move to Part2 when complete
```

---

## 📊 Learning Outcomes

By completing this exercise, you will:

✅ **Technical Skills:**
- Implement One-Class SVM for anomaly detection
- Engineer features from database access logs
- Train personalized per-user detection models
- Tune SVM hyperparameters (kernel, nu)

✅ **Security Skills:**
- Identify database access anomalies
- Detect reconnaissance and exfiltration attacks
- Recognize credential compromise indicators
- Design operational alert workflows

✅ **Operational Skills:**
- Balance detection vs false positives
- Build tiered alert systems
- Create analyst investigation procedures
- Design model retraining strategies

---

## 🔗 Related Resources

**Chapter Content:**
- Section 8.4: Anomaly Detection Algorithms
- One-Class SVM Theory
- Security Applications

**Datasets:**
- LANL Authentication Dataset (real alternative)
- Simulated enterprise logs (this exercise)

**Tools:**
- scikit-learn: `sklearn.svm.OneClassSVM`
- pandas: Data manipulation
- matplotlib/seaborn: Visualization

---

## ❓ Troubleshooting

### Common Issues:

**Issue 1**: "No module named sklearn"
```bash
pip install scikit-learn
```

**Issue 2**: Notebooks won't open
- Ensure Jupyter is installed: `pip install jupyter`
- Navigate to correct directory

**Issue 3**: Data generation takes too long
- Reduce days from 28 to 14 in Part 1
- Reduce queries_per_day ranges

**Issue 4**: Memory errors
- Process one admin at a time
- Use sampling for large datasets

---

## 📝 Progress Tracker

- [x] Part 1: Data Generation ✅
- [ ] Part 2: Feature Engineering ⏳ 
- [ ] Part 3: One-Class SVM Training 📋
- [ ] Part 4: Attack Detection 📋
- [ ] Part 5: Operational Deployment 📋

**Total Progress**: 20% Complete

---

## 🎓 Next Steps

1. **Open** `Part1_Data_Generation.ipynb`
2. **Run** all cells to generate data
3. **Verify** data files created in `data/` directory
4. **Move to** `Part2_Feature_Engineering.ipynb` (coming next)

---

**Instructor Note**: This modular structure allows students to:
- Complete exercise in multiple sessions
- Focus on specific phases
- Rerun parts without redoing everything
- Experiment with variations easily

**Created**: December 2025  
**Version**: 1.0  
**Course**: AI/ML Techniques in Cybersecurity
