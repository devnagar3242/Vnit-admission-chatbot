# VNIT Admission Assistant 🍊


A hybrid **Machine Learning + Rule-Based Admission Chatbot** for VNIT Nagpur.

The chatbot answers common admission-related queries such as programs, eligibility, admission process, entrance exams, cutoffs, fees, documents, counselling, and placements.

The system also provides basic explainability by showing the predicted intent, confidence score, and whether the final intent came from the Machine Learning model or rule-based system.

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-TF--IDF%20%2B%20LogReg-orange)
![Flask](https://img.shields.io/badge/Flask-Web%20App-lightgrey)

---

## Live Demo

VNIT Admission Assistant:

https://vnit-admission-chatbot.onrender.com/

---

## Project Objective

Students usually have many questions during the admission process, such as:

- What programs are available?
- What is the eligibility for B.Tech or M.Tech?
- What entrance exam is required?
- What is the cutoff?
- What documents are required?
- What is the fee structure?
- How does counselling work?
- What are the placement opportunities?

The main goal of this project is to provide these answers through a simple chatbot interface.

---

## How It Works

The chatbot uses a **hybrid intent-recognition approach**.

### 1. Text Vectorization

The user question is first converted into numerical features using **TF-IDF**.

The project uses:

- Word n-grams: **1–2**
- Character n-grams: **3–5**

Word n-grams help the model understand words and short phrases.

Character n-grams also help with some spelling mistakes and text variations.

Example:

```text
"What is the M.Tech fee?"
        ↓
      TF-IDF
        ↓
Numerical Features
```

---

### 2. Intent Classification

The numerical features are passed to a **Logistic Regression classifier**.

The classifier predicts one of **9 different intents**.

Example:

```text
User Question:
"What is the M.Tech fee?"

Predicted Intent:
fees
```

The model also returns probability scores which are used as confidence scores.

---

### 3. Rule-Based Recognition

A second system also checks the user query using:

- Keywords
- Regular Expressions
- Program names
- Specialization names

The rule-based recognizer also extracts information such as:

```text
Program = M.Tech
Specialization = CSE
```

---

### 4. Confidence-Based Fallback

The Machine Learning model is used first for the final intent prediction.

If ML confidence is **0.40 or higher**, the ML intent is used.

If ML confidence is below **0.40**, the rule-based intent is used.

```text
ML Confidence >= 0.40
        ↓
Use ML Intent

ML Confidence < 0.40
        ↓
Use Rule-Based Intent
```

---

### 5. Response Generation

After identifying the intent, program, and specialization, the information is passed to the **Response Generator**.

The Response Generator selects the appropriate handler and generates the final answer.

Example:

```text
Intent = fees
Program = m_tech
        ↓
Response Generator
        ↓
M.Tech Fee Information
```

It also provides suggested follow-up questions.

---

## Architecture

```text
                         User Query
                             |
              +--------------+--------------+
              |                             |
              v                             v
     MLIntentClassifier              IntentRecognizer
     TF-IDF + Logistic               Regex + Keywords
        Regression                   + Entity Extraction
              |                             |
              |                      Program/Specialization
              |
        Confidence >= 0.40?
             /        \
           Yes         No
            |           |
       ML Intent    Rule Intent
             \         /
              \       /
               v     v
          ResponseGenerator
                 |
                 v
            Final Answer
                 |
                 v
           SQLite Logging
                 |
                 v
               User
```

---

## Dataset

The project uses a custom labeled dataset stored in:

```text
data/training_data.json
```

The dataset contains:

- **257 labeled example questions**
- **9 intent categories**

The intents are:

1. `program_info`
2. `eligibility`
3. `admission_process`
4. `entrance_exam`
5. `documents`
6. `fees`
7. `placement`
8. `counseling`
9. `general`

Example training data:

```text
"What is the M.Tech fee?"        -> fees

"Am I eligible for B.Tech?"      -> eligibility

"What is the CSE cutoff?"        -> entrance_exam

"Average package at VNIT?"       -> placement

"What documents are required?"   -> documents
```

The Machine Learning model learns patterns from these labeled examples.

---

## Machine Learning Model

The project uses:

### TF-IDF

TF-IDF converts text into numerical features so that the Machine Learning model can process user questions.

### Logistic Regression

Logistic Regression is used as the classification algorithm.

It predicts which intent the user question belongs to.

Example:

```text
Question
   ↓
TF-IDF
   ↓
Numerical Features
   ↓
Logistic Regression
   ↓
Intent Prediction
```

---

## Model Evaluation

The model is evaluated using **5-fold cross-validation**.

In 5-fold cross-validation, the dataset is divided into 5 parts.

The model is trained and tested 5 times, using a different part for testing each time.

### Current Results

| Metric | Value |
|---|---:|
| Number of Intents | 9 |
| Training Examples | 257 |
| 5-Fold CV Accuracy | **0.82** |
| 5-Fold Macro F1 Score | **0.82** |

Run the evaluation using:

```bash
python evaluate.py
```

---

## Main Features

- Hybrid ML + rule-based chatbot
- TF-IDF text vectorization
- Logistic Regression intent classifier
- 9 admission-related intents
- Word n-gram features
- Character n-gram features
- Confidence-based fallback
- Regex and keyword-based recognition
- Program extraction
- Specialization extraction
- Admission information responses
- Eligibility information
- Entrance exam information
- Fee information
- Document information
- Placement information
- Counselling information
- Suggested follow-up questions
- Conversation history
- SQLite database logging
- User query analytics
- CLI chatbot
- Flask-based web interface
- Admin analytics interface
- Web deployment using Render

---

## Database

The project uses **SQLite** for storing chatbot interaction data.

The main database tables are:

### Users

Stores:

- User ID
- User name
- First interaction
- Last interaction
- Total queries

### Conversations

Stores:

- User message
- Bot response
- Intent
- Program
- Specialization
- Confidence score
- Timestamp

This data can be used for basic analytics.

---

## Technology Stack

### Programming Language

- Python

### Machine Learning

- Scikit-learn
- TF-IDF
- Logistic Regression

### NLP / Text Processing

- Word n-grams
- Character n-grams
- Regular Expressions
- Keyword matching

### Backend

- Flask

### Database

- SQLite

### Frontend

- HTML
- CSS
- JavaScript

### Development Tools

- VS Code
- Git
- GitHub

### Deployment

- Render

---

## Project Structure

```text
VNIT-Admission-Chatbot/
│
├── config/
│   ├── __init__.py
│   └── config.py
│
├── data/
│   ├── training_data.json
│   ├── admission_data.json
│   ├── programs.json
│   ├── faqs.json
│   └── intent_model.joblib
│
├── src/
│   ├── __init__.py
│   ├── ml_intent_classifier.py
│   ├── intent_recognition.py
│   ├── response_generator.py
│   ├── chatbot.py
│   ├── database.py
│   └── utils.py
│
├── interfaces/
│   ├── __init__.py
│   ├── web_app.py
│   ├── cli.py
│   └── admin.py
│
├── templates/
│   └── index.html
│
├── static/
│   ├── app.js
│   ├── style.css
│   └── vnit_logo.jpg
│
├── tests/
│   ├── __init__.py
│   └── test_chatbot.py
│
├── main.py
├── admin.py
├── evaluate.py
├── requirements.txt
├── render.yaml
├── wsgi.py
└── README.md
```

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/devnagar3242/Vnit-admission-chatbot.git
```

Move into the project directory:

```bash
cd Vnit-admission-chatbot
```

---

### 2. Create Virtual Environment

```bash
python -m venv .venv
```

For Windows:

```bash
.venv\Scripts\activate
```

---

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Run the Project

### CLI Chatbot

Run:

```bash
python main.py
```

The chatbot will ask for your name and then you can start asking questions.

Example:

```text
What is the M.Tech fee?

What is B.Tech eligibility?

What documents are required?

What is the CSE cutoff?

How does JoSAA counselling work?
```

---

## Run Web Application

Run:

```bash
python interfaces/web_app.py
```

Then open:

```text
http://127.0.0.1:5000
```

in your browser.

---

## Admin Analytics

Run:

```bash
python interfaces/admin.py
```

The admin interface can be used to view information such as:

- Registered users
- Total queries
- Conversation records
- Most asked intents
- Most asked programs

---

## ML Evaluation

Run:

```bash
python evaluate.py
```

This evaluates the intent classification model using 5-fold cross-validation.

---

## Example Chatbot Flow

```text
User:
"What is M.Tech fee?"

        ↓

TF-IDF converts question into features

        ↓

Logistic Regression predicts:

Intent = fees
Confidence = 0.82

        ↓

Rule-Based Recognizer extracts:

Program = m_tech

        ↓

Response Generator

        ↓

M.Tech Fee Information

        ↓

Conversation stored in SQLite
```

---

## Future Improvements

- [ ] Increase the training dataset
- [ ] Add more real-world admission questions
- [ ] Improve Hinglish query handling
- [ ] Improve spelling-error handling
- [ ] Compare Logistic Regression with SVM
- [ ] Compare Logistic Regression with Naive Bayes
- [ ] Improve confidence threshold selection
- [ ] Improve program and intent conflict handling
- [ ] Analyze low-confidence queries
- [ ] Add sentence-transformer embeddings
- [ ] Add semantic search
- [ ] Add RAG using official VNIT documents
- [ ] Automatically update admission information
- [ ] Improve analytics dashboard
- [ ] Add more automated tests

---

## Author

### Devendra Nagar

**M.Tech CSE**  
Visvesvaraya National Institute of Technology, Nagpur

**GitHub:**  
https://github.com/devnagar3242

**Project Repository:**  
https://github.com/devnagar3242/Vnit-admission-chatbot

---

## Contact

For project-related information, issues, or improvements, please use the GitHub repository:

https://github.com/devnagar3242/Vnit-admission-chatbot