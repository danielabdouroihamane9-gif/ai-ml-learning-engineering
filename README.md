
# AI & ML Learning Journey

A structured, practical learning journey toward becoming an **AI/ML Engineer**, with strong foundations in **Data Science, Machine Learning, and Software Engineering**.

This repository documents my progress, exercises, experiments, notes, and independent projects as I develop the skills to analyse real-world data, build machine learning models, and deploy AI-powered applications.

## 🎯 Learning Objectives

My goal is to develop the ability to:

- Write clean and maintainable Python code.
- Manipulate, clean, and analyse real-world datasets.
- Perform Exploratory Data Analysis (EDA).
- Apply statistics and probability to data problems.
- Query and manage relational databases using SQL.
- Build, train, and evaluate machine learning models.
- Apply feature engineering and model optimization techniques.
- Understand deep learning, neural networks, and NLP.
- Work with LLMs, embeddings, RAG, and AI agents.
- Deploy and maintain production-ready machine learning systems.
- Integrate AI/ML capabilities into full-stack applications.

## 🛠️ Development Environment

This repository uses the following tools and technologies:

| Tool             | Purpose                                  |
| ---------------- | ---------------------------------------- |
| Python           | Primary programming language             |
| VS Code          | Main development environment             |
| Jupyter Notebook | Data exploration and experimentation     |
| NumPy            | Numerical computing                      |
| Pandas           | Data manipulation and analysis           |
| Matplotlib       | Data visualization                       |
| Seaborn          | Statistical visualization                |
| Scikit-learn     | Classical machine learning               |
| Git              | Version control                          |
| GitHub           | Repository hosting and progress tracking |
| PostgreSQL       | Relational database practice             |
| Google Colab     | Optional cloud computing and GPU access  |

**Development approach:** VS Code is the primary workspace. Jupyter Notebooks are used inside VS Code for interactive learning, data analysis, and experimentation. Python modules (`.py`) are used for reusable code and application development.

## 📚 Learning Roadmap

The learning sequence is organized into progressive stages.

### Stage 1 — NumPy

- Arrays and dimensions
- Array indexing and slicing
- Array operations
- Broadcasting
- Vectorization
- Aggregation and numerical operations

### Stage 2 — Pandas

- Series and DataFrames
- Reading CSV and JSON files
- Data selection and filtering
- Handling missing values
- Removing duplicates
- Data cleaning and transformation
- Grouping and aggregation
- Merging datasets

### Stage 3 — Data Visualization and EDA

- Matplotlib fundamentals
- Histograms and bar charts
- Scatter plots and box plots
- Understanding distributions
- Identifying outliers
- Exploring relationships between variables
- Communicating findings through visualizations

### Stage 4 — Statistics and Probability

- Descriptive statistics
- Mean, median, and mode
- Variance and standard deviation
- Probability fundamentals
- Probability distributions
- Sampling
- Correlation and causation
- Confidence intervals
- Hypothesis testing

### Stage 5 — SQL and Databases

- Relational database fundamentals
- SQL queries
- Filtering and sorting
- Joins
- Aggregations
- Subqueries
- Data extraction and management

### Stage 6 — Supervised Machine Learning

- Introduction to machine learning
- Machine learning workflow
- Regression
- Classification
- Training and testing datasets
- Linear regression
- Logistic regression
- Model predictions

### Stage 7 — Model Evaluation and Feature Engineering

- Evaluation metrics
- MAE, MSE, RMSE, and R²
- Accuracy, precision, recall, and F1-score
- Confusion matrices
- Train/validation/test splits
- Data leakage
- Feature engineering
- Encoding and scaling
- Cross-validation
- Overfitting and underfitting

### Stage 8 — Advanced Classical Machine Learning

- Decision trees
- Random forests
- Gradient boosting
- XGBoost
- Regularization
- Model comparison and optimization

### Stage 9 — Unsupervised Learning and Time Series

- Clustering
- K-means
- Dimensionality reduction
- Principal Component Analysis (PCA)
- Time-series fundamentals
- Time-series analysis and modelling

### Stage 10 — Practical Projects and Competitions

- Working with real-world datasets
- Complete data science workflows
- Machine learning competitions
- Independent model development
- Model comparison
- Error analysis
- Reproducible experiments

### Stage 11 — Machine Learning Engineering and Deployment

- Model serialization
- Building prediction APIs
- FastAPI
- Docker
- Cloud deployment
- Model serving
- Kubernetes fundamentals

### Stage 12 — Deep Learning

- Neural network fundamentals
- Perceptrons and layers
- Activation and loss functions
- Backpropagation
- PyTorch
- Convolutional Neural Networks (CNNs)
- Transfer learning

### Stage 13 — NLP and Modern AI

- Text preprocessing
- Natural Language Processing
- Word and sentence embeddings
- Attention mechanisms
- Transformers
- Large Language Models (LLMs)
- Retrieval-Augmented Generation (RAG)
- AI agents

### Stage 14 — Production AI and MLOps

- Experiment tracking
- Model versioning
- Model evaluation and monitoring
- Data and model drift
- Model deployment pipelines
- CI/CD
- Inference optimization
- Model maintenance and retraining

## 🎓 Learning Resources

This journey combines three complementary programs rather than treating them as independent courses.

| Program                                                | Primary Focus                                             |
| ------------------------------------------------------ | --------------------------------------------------------- |
| Matsuo Lab — GCI World 2026                           | Data science and machine learning foundations             |
| WorldQuant University — Data Science Fundamentals Lab | Practical data science and real-world datasets            |
| DataTalks.Club — Machine Learning Zoomcamp 2026       | Machine learning development, engineering, and deployment |

## 📁 Repository Structure

```text
ai-ml-learning-journey/
├── 01_numpy/
│   ├── notebooks/
│   ├── exercises/
│   └── README.md
│
├── 02_pandas/
│   ├── notebooks/
│   ├── exercises/
│   └── README.md
│
├── 03_data_visualization/
│   ├── notebooks/
│   ├── exercises/
│   └── README.md
│
├── 04_statistics_probability/
│   ├── notebooks/
│   ├── exercises/
│   └── README.md
│
├── 05_sql/
│   ├── queries/
│   └── README.md
│
├── 06_machine_learning/
│   ├── regression/
│   ├── classification/
│   ├── evaluation/
│   ├── feature_engineering/
│   └── README.md
│
├── 07_unsupervised_learning/
│
├── 08_deep_learning/
│
├── 09_nlp_llms/
│
├── requirements.txt
├── .gitignore
└── README.md
```

The structure will evolve as the learning journey progresses.

## ⚙️ Environment Setup

### 1. Clone the repository

```bash
git clone https://github.com/danielabdouroihamane9-gif/ai-ml-learning-engineering.git
cd ai-ml-learning-journey
```

### 2. Create a virtual environment

Windows PowerShell:

```powershell
py -3.13 -m venv .venv
```

### 3. Activate the environment

```powershell
.\.venv\Scripts\Activate.ps1
```

### 4. Install the required libraries

```bash
python -m pip install --upgrade pip

pip install numpy pandas matplotlib seaborn jupyter ipykernel scikit-learn
```

### 5. Register the Jupyter kernel

```bash
python -m ipykernel install --user --name ai-ml-learning --display-name "Python (AI-ML Learning)"
```

### 6. Select the kernel in VS Code

Open a `.ipynb` notebook and select:

`Python (AI-ML Learning)`
