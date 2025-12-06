# Exercise 8.4B: One-Class SVM for Database Access Anomaly Detection

## 📚 Complete Step-by-Step Implementation

**Duration**: 3-4 hours  
**Difficulty**: Intermediate-Advanced

---

## 🎯 Exercise Overview

This exercise teaches you how to use **One-Class SVM** to detect anomalous database access patterns that could indicate:
- Compromised administrator credentials
- Insider threat activity  
- Reconnaissance attacks
- Mass data exfiltration

---

## 🔐 Problem Description

### The Security Challenge

Database administrators have privileged access to an organization's most sensitive data—customer information, financial records, system configurations, and audit logs. While this access is necessary for their roles, it also creates significant security risks:

**Threat Scenarios:**
1. **Credential Compromise**: Attacker steals admin credentials through phishing, malware, or social engineering
2. **Insider Threat**: Malicious employee abuses legitimate access to steal data or cause damage
3. **Account Takeover**: External attacker gains unauthorized access and mimics normal behavior
4. **Lateral Movement**: Compromised low-privilege account escalates to admin access

**Why Traditional Security Fails:**
- **Rule-based systems** can't catch novel attack patterns or subtle behavioral changes
- **Signature detection** requires known attack signatures (useless for zero-day threats)
- **Threshold alerts** (e.g., "more than 100 queries") are easily evaded by sophisticated attackers
- **Manual review** of millions of database queries is operationally infeasible

### The Machine Learning Solution

We use **One-Class SVM** (Support Vector Machine) for **anomaly detection** because:

✅ **Learns normal behavior**: Trains only on legitimate admin activity (no attack examples needed)  
✅ **Personalized models**: Each admin has unique access patterns—one model per user  
✅ **Detects unknown attacks**: Flags deviations from normal, regardless of attack type  
✅ **Handles high-dimensional data**: Works with 20+ behavioral features (query types, timing, volume, sensitivity)

**How It Works:**
1. **Training Phase**: Learn decision boundary around normal admin behavior using only baseline data
2. **Detection Phase**: Score new database queries—inside boundary = normal, outside = anomaly
3. **Alert Tier**: Classify anomalies by confidence (Critical/High/Medium/Low) based on distance from boundary
4. **Investigation**: Security analysts review flagged activities using structured playbooks

### Real-World Context

**Scenario**: Enterprise with 3 database administrators:
- **Alice** (Data Analyst): Runs reports, queries customer/transaction data during business hours
- **Bob** (System Admin): Maintains systems, accesses audit logs and config, works extended hours
- **Charlie** (Developer): Tests code, uses complex JOINs on product/order data, flexible schedule

**Attack Simulation**: After 28 days of normal activity, we inject realistic attacks:
- **Day 29, Alice account**: Reconnaissance attack (systematic SELECT * queries across all tables)
- **Day 29, Bob account**: Mass data extraction (10-20× normal row counts from sensitive tables)
- **Day 29, Charlie account**: Credential theft (unusual 3 AM access to atypical tables)

**Detection Goal**: Identify attacks with high recall (catch most attacks) while maintaining low false positive rate (minimize alert fatigue for analysts).

### What You'll Build

This 5-part exercise walks through the complete machine learning security pipeline:

1. **Data Generation**: Create realistic database access logs with normal behavior + attacks
2. **Feature Engineering**: Extract 24 behavioral features (query patterns, timing, table access, resource usage)
3. **Model Training**: Train personalized One-Class SVM models (Linear vs RBF kernels) per admin
4. **Attack Detection**: Evaluate detection performance with ROC/PR curves, confusion matrices, error analysis
5. **Operational Deployment**: Design tiered alert workflow, investigation playbooks, model lifecycle management

**Learning Outcomes**: Understand anomaly detection theory, apply One-Class SVM to security problems, balance detection vs false positives, design operational ML systems for SOC (Security Operations Center) environments.

---

## 📂 Project Structure

```
Exercise_8_4B_Database_Anomaly_Detection/
├── Part1_Data_Generation.ipynb
├── Part2_Feature_Engineering.ipynb
├── Part3_OneClass_SVM_Training.ipynb
├── Part4_Attack_Detection.ipynb
├── Part5_Operational_Deployment.ipynb
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

## 📋 Part 1: Data Generation

**What You'll Build:**
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

## 📋 Part 2: Feature Engineering

**What You'll Build:**

### Phase 1: Per-Query Features
Extract features from each database query:
- Query complexity metrics
- Table access patterns
- Temporal features
- Resource consumption

### Phase 2: Aggregated Behavioral Features
Aggregate per-admin over time windows (daily):
- Query type distribution
- Table access frequency
- Timing patterns
- Anomaly indicators

### Phase 3: Feature Normalization
Prepare features for One-Class SVM:
- StandardScaler for consistent scales
- Handle missing values
- Create training/test splits

**Expected Output:**
- `data/features_train_admin_alice.csv`
- `data/features_train_admin_bob.csv`
- `data/features_train_admin_charlie.csv`
- `data/features_test_admin_*.csv`
- Feature correlation matrix
- Feature distribution plots

---

## 📋 Part 3: One-Class SVM Training

**What You'll Build:**

### Phase 1: Baseline Model Training
- Train One-Class SVM per administrator on normal data
- Compare Linear and RBF kernels
- Tune `nu` parameter (expected anomaly fraction)

### Phase 2: Model Evaluation
- Analyze support vectors
- Validate on held-out normal data
- Check false positive rate on known-good queries

**Expected Output:**
- `models/oneclass_svm_admin_alice.pkl`
- `models/oneclass_svm_admin_bob.pkl`
- `models/oneclass_svm_admin_charlie.pkl`
- `models/model_configs.json`
- Training metrics report

---

## 📋 Part 4: Attack Detection

**What You'll Build:**

### Phase 1: Score Attack Scenarios
- Score attack logs with trained models
- Analyze anomaly scores
- Compare to normal baseline scores

### Phase 2: Performance Evaluation
- Compute TPR, FPR, Precision, Recall
- Generate ROC curves per admin
- Create confusion matrices

### Phase 3: False Positive Analysis
- Investigate false positives
- Understand model limitations
- Document edge cases

**Expected Output:**
- `results/detection_performance_report.csv`
- `results/roc_curves.png`
- `results/precision_recall_curves.png`
- `results/confusion_matrices.png`

---

## 📋 Part 5: Operational Deployment

**What You'll Build:**

### Phase 1: Alert Workflow Design
- Define High/Medium/Low/Critical confidence tiers
- Design automated response actions
- Create analyst investigation procedures

### Phase 2: Monitoring Dashboard
- Design real-time anomaly feed
- Implement alert prioritization
- Plan historical trend analysis

### Phase 3: Model Lifecycle
- Define retraining procedures
- Design drift detection strategy
- Plan feedback incorporation

**Expected Output:**
- `results/investigation_playbook.txt`
- Alert workflow definitions
- Deployment architecture recommendations

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

## 🎓 How to Complete This Exercise

1. **Start with** `Part1_Data_Generation.ipynb` to create synthetic database logs
2. **Continue to** `Part2_Feature_Engineering.ipynb` to extract behavioral features
3. **Train models in** `Part3_OneClass_SVM_Training.ipynb` with different kernels
4. **Evaluate detection in** `Part4_Attack_Detection.ipynb` with ROC/PR analysis
5. **Design deployment in** `Part5_Operational_Deployment.ipynb` for production use

---

**Instructor Note**: This modular structure allows students to:
- Complete exercise in multiple sessions
- Focus on specific phases
- Rerun parts without redoing everything
- Experiment with variations easily

**Created**: December 2025  
**Version**: 1.0  
**Course**: AI/ML Techniques in Cybersecurity
