# Unit 1: Email Spam Classification Agent — Implementation & PEAS Analysis

> **Notebook Reference:** [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb)  
> **Dataset Reference:** [`datasets/one.csv`](file:///c:/AI%20LAB%20MAIN/UNIT_1/datasets/one.csv)  
> **Course Outcomes Addressed:**  
> - **CO1:** Implement and analyse intelligent agents, state-space representations and classical search algorithms.  
> - **CO5:** Design, verify and communicate AI solutions using responsible and human-supervised AI workflows.

---

## 📌 1. Experiment Problem Statement

The objective of the experiment in [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb) is to design and evaluate an **Intelligent Text Classification Agent** that receives text documents (email bodies) and automatically determines whether the email is **Spam (1)** or **Not Spam / Ham (0)**.

In this initial lab iteration, the system operates as a **batch test program and standalone prototype** within a Jupyter Notebook environment. It processes historical email records from a static dataset (`datasets/one.csv`), vectorizes text features using **TF-IDF**, trains a **Logistic Regression** decision model, and executes inference on standalone sample text strings without being connected to a live email server (IMAP/SMTP).

---

## 🔬 2. Implementation Overview (`one.ipynb` Analysis)

The experiment implemented in [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb) follows a structured 5-stage AI pipeline:

```
[Raw CSV Dataset] ──► [Data Cleaning] ──► [TF-IDF Vectorizer] ──► [Logistic Regression] ──► [Console Classification Output]
```

### Stage 1: Data Ingestion & Preprocessing
- **Source:** [`datasets/one.csv`](file:///c:/AI%20LAB%20MAIN/UNIT_1/datasets/one.csv)
- **Cleaning:** Removes missing values (`dropna()`), filters binary target labels (`label ∈ {0, 1}`), and separates input text (`X = df['email']`) from ground-truth target (`y = df['label']`).

### Stage 2: Feature Transformation (Text to Vector)
- **Transformer:** `scikit-learn` `TfidfVectorizer(stop_words='english', lowercase=True)`
- **Vocabulary Size:** Extracted **33,805 numerical features** representing term frequency-inverse document frequency weights across the corpus.

### Stage 3: Model Training & Train/Test Split
- **Dataset Partition:** 80% Training / 20% Testing (`test_size=0.2`, `random_state=11`)
- **Classifier Model:** `LogisticRegression(max_iter=1000, solver='lbfgs')`

### Stage 4: Empirical Evaluation Results
- **Overall Model Accuracy:** **`96.50%`**

#### Classification Breakdown
| Class | Label Description | Precision | Recall | F1-Score | Support |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **0** | Not Spam (Ham) | 0.96 | 1.00 | 0.98 | 497 |
| **1** | Spam | 1.00 | 0.80 | 0.89 | 103 |
| **Overall** | **Weighted Average** | **0.97** | **0.96** | **0.96** | **600** |

> [!NOTE]
> **Key Insight:** The model achieves **100% Precision (1.00)** on Spam detection. This means zero false positives were observed in the test set (no legitimate email was mistakenly classified as spam), which is critical for email filtering safety.

### Stage 5: Inference on Sample Test Data (Test Harness Execution)
The notebook tests the trained agent on a raw sample text prompt:
- **Input Text String:**  
  `"Click here to claim your prize. You have won a free gift card! Act now to receive your reward."`
- **Vector Transformation:** Transformed via `transformer.transform([Email_data_spam])`
- **Agent Decision:** Predicts label `1`
- **Output Action:** Prints to console:  
  `Predicted label for the input email: Spam`

---

## 🤖 3. PEAS Analysis of the Notebook Test Agent

Evaluating the test program in [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb) under the PEAS framework:

| PEAS Element | Test Program Agent Formulation (`one.ipynb`) |
| :--- | :--- |
| **Performance Measure** | - High Classification Accuracy (**96.50%** achieved)<br>- High Precision on Spam (1.00 to avoid misclassifying important emails)<br>- F1-score on Spam (0.89)<br>- Low computational latency for vector transformation & inference |
| **Environment** | - Offline Jupyter Notebook Python runtime environment<br>- Static CSV dataset (`datasets/one.csv`) with 3,000 email records<br>- 80/20 train/test evaluation split<br>- Non-realtime test harness (hardcoded string inputs, no live network connection) |
| **Actuators** | - Console stdout print stream (`print("Predicted label for the input email: Spam")`) <br>- Binary return signal (`y_pred ∈ {0, 1}`) returned to memory |
| **Sensors** | - Pandas CSV file reader (`pd.read_csv`) reading raw email string column `df['email']`<br>- `TfidfVectorizer` mapping string input to 33,805-dimensional feature vectors |

### Environment Properties of the Notebook Agent

- **Observability:** **Fully Observable** (the agent has complete access to the input text string and TF-IDF feature vector during inference).
- **Agent Type:** **Single-agent** (the classification script operates independently).
- **Determinism:** **Deterministic** (with fixed model weights and `random_state=11`, identical input text yields identical TF-IDF vectors and classification outputs).
- **Episodic vs. Sequential:** **Episodic** (each email string classification is an independent episode; classifying email $N$ does not change the classification of email $N+1$).
- **Static vs. Dynamic:** **Static** (the dataset and model weights do not change while the agent makes a decision).
- **Discrete vs. Continuous:** **Discrete** (binary target classes `{0, 1}` and discrete text token vocabulary).

---

## 🔄 4. Notebook Test Agent vs. Production Live Email Agent

Because [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb) is a test program, its PEAS design differs significantly from a full production email security system:

| Aspect | Notebook Test Agent (`one.ipynb`) | Production Live Email Security Agent |
| :--- | :--- | :--- |
| **Environment** | Static offline notebook, CSV file | Dynamic email server (IMAP/SMTP/Exchange), incoming live network stream |
| **Sensors** | Raw email body string variable | Full MIME parser, IP reputation sensor, DKIM/SPF auth headers, attachment sandbox |
| **Actuators** | Terminal `print()` statement | Move to Junk folder, quarantine email, block sender IP, alert security admin |
| **Learning Mode** | Batch offline training (fit once) | Online / Continual learning with user feedback ("Report Spam / Not Spam") |

---

## 💡 5. AI Assistant Integration & Comparison

### Prompt Sent to Generative AI Assistant:
> *"Formulate a PEAS analysis for an Email Spam Classification script implemented using Python, TF-IDF, and Logistic Regression in a Jupyter Notebook."*

### AI Assistant Generated PEAS:
- **Performance Measure:** Accuracy, precision, recall, execution speed.
- **Environment:** Jupyter Notebook workspace, Python environment.
- **Actuators:** Displaying output in notebook cell execution output.
- **Sensors:** Input dataset file and test string variables.

### Comparative Evaluation & Justifications:

1. **Test Harness Recognition:**  
   The AI assistant correctly identified that the agent's actuators are confined to notebook cell output displays rather than mail server folder actions.
2. **Precision vs. Recall Trade-off:**  
   The AI assistant initially grouped "accuracy" and "recall" equally. Human verification of [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb) output demonstrated that **Spam Precision (1.00)** is the most critical metric because false positives (marking legitimate mail as spam) carry a much higher penalty than false negatives.
3. **Feature Space Representation:**  
   Human analysis explicitly detailed how the sensor mechanism converts raw text into a 33,805-dimensional TF-IDF sparse matrix, whereas the AI assistant abstracted this simply as "text input".

---

## 📝 6. Conclusion & Verification Summary

The experiment in [`one.ipynb`](file:///c:/AI%20LAB%20MAIN/UNIT_1/one.ipynb) successfully demonstrates a foundational **Intelligent Agent** for text classification. The agent perceives raw text via TF-IDF vector sensors, makes decisions via Logistic Regression weights, and acts by outputting classification labels.

- **Verified Accuracy:** 96.50% overall test accuracy.
- **Verified Spam Precision:** 100% (0 false positives on test batch).
- **Execution Context:** Confirmed as an offline test harness program.
