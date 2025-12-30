# MiniProject – Foundations of Knowledge Graphs

This mini-project builds and evaluates a Knowledge Graph–based fact checking pipeline using the FOKG dataset.  
The system converts reified RDF statements into structured triples, learns veracity patterns, and produces probability scores suitable for GERBIL evaluation.

Two models are implemented:

1. Baseline Model – frequency-based heuristic  
2. ML Model – Logistic Regression with engineered graph/statistical features  

Both models generate a GERBIL-compatible TTL output file.

Adjust notebook or script paths if you change folder names.

---

## 2. Prerequisites

- Python 3.10+ (3.9 also works)
- pip (latest recommended)
- Jupyter Notebook or JupyterLab (optional but recommended)
- rdflib, pandas, numpy, scikit-learn

---

## 3. Environment Setup

### 3.1 Create virtual environment

From project root:

python -m venv venv

Activate:

macOS/Linux:
source venv/bin/activate

Windows:
venv\Scripts\activate

### 3.2 Install dependencies

pip install -r requirements.txt

or manually:

pip install rdflib pandas numpy scikit-learn jupyter

### 3.3 Dataset placement

Place the two dataset files in:

data/KG-2022-train.nt  
data/KG-2022-test.nt  

Or update the notebooks/scripts with your custom paths.

---

## 4. Running the Baseline Notebook

Notebook: notebooks/01_FoundationsOfKnowledgeGraphs_Baseline.ipynb

This notebook:

- Loads train/test RDF files  
- Parses reified statements (rdf:Statement)  
- Computes frequency-based veracity statistics  
- Scores each test fact  
- Exports result.ttl in GERBIL format  

### Launch:

jupyter notebook  
Open the baseline notebook and run all cells top-to-bottom.

### Output:

A file named result.ttl in the project root.

Format example:

<fact_uri> <http://swc2017.aksw.org/hasTruthValue> "0.659794"^^<http://www.w3.org/2001/XMLSchema#double> .

---

## 5. Running the ML Notebook

Notebook: notebooks/02_FoundationsOfKnowledgeGraphs_ML.ipynb

This notebook:

- Loads the same RDF datasets  
- Computes statistical and structural features:
  * predicate truth frequency
  * (subject, predicate) frequency and counts
  * (predicate, object) frequency and counts
  * subject and object occurrence degree
- Builds numeric feature vectors  
- Trains a Logistic Regression classifier with scaling  
- Prints training ROC AUC  
- Scores test facts  
- Exports result_ml.ttl  

### Launch:

jupyter notebook  
Open 02_FoundationsOfKnowledgeGraphs_ML.ipynb and run all cells.

### Output:

result_ml.ttl in the project root.

---

## 6. Running ML Pipeline as a Script

A standalone Python script version is included:

python train_ml_veracity.py

It will:

- Load train/test RDF files  
- Train the ML model  
- Print ROC AUC  
- Produce result_ml.ttl  

---

## 7. Using GERBIL for Evaluation

1. Go to GERBIL web interface  
2. Select: Fact Checking  
3. Upload: result.ttl (baseline) OR result_ml.ttl (ML model)  
4. Select dataset: FOKG 2024 Test (or matching name)  
5. Record:
   - ROC AUC  
   - Any extra metrics  
   - Screenshot for your report  

---

## 8. Troubleshooting

- “File not found”: Ensure .nt files are inside data/  
- rdflib parse errors: Check file format is N-Triples (.nt), not Turtle  
- Missing modules: pip install -r requirements.txt  
- Low GERBIL AUC: Ensure each test fact has exactly one triple in your result file  

---

## 9. Reproducibility

- The ML model is deterministic (no random seed needed)  
- For strict reproducibility:
  - Fix random_state in LogisticRegression  
  - Capture dependency versions with:
    pip freeze > requirements.txt  

---

This project demonstrates how symbolic RDF knowledge graphs can encode enough structure to evaluate factual claims using both heuristic and machine learning methods.
