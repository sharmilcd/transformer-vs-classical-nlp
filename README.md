# Transformer vs Classical NLP  
### A Diagnostic Study on Sentiment Classification

---

## 📌 Objective

This project compares classical NLP models with transformer-based models for sentiment classification, focusing on:

- When simple models fail  
- What transformers actually improve  
- Whether failures arise from data limitations or model limitations  

---

## ⚙️ Models Used

### 1. Naive Bayes (NB)
- CountVectorizer + MultinomialNB  
- Assumes word independence  
- Fast but simplistic  

### 2. Logistic Regression (LR)
- TF-IDF features (~10,000 dimensions)  
- Learns weighted combination of words  
- Strong classical baseline  

### 3. DistilBERT
- Pretrained transformer model  
- Fine-tuned on dataset  
- Captures contextual relationships  

---

## 📊 Dataset

- IMDb dataset (subset: ~3000–5000 samples)  
- Binary sentiment classification (positive / negative)  

---

## 📈 Results

| Model | Accuracy | F1 Score |
|------|--------|---------|
| Naive Bayes | 0.8383 | 0.8289 |
| Logistic Regression | 0.8433 | 0.8469 |
| DistilBERT | 0.8883 | 0.8885 |

DistilBERT Training Loss:
- Step 50 → 0.5479  
- Step 100 → 0.3400  
- Step 150 → 0.3337  

---

## 🔬 Diagnostic Test Suite

test_cases = [
    ("This is a good movie", 1),
    ("This is a bad movie", 0),
    ("Amazing acting and great story", 1),
    ("Terrible plot and boring film", 0),
    ("It was a long, long, long, long, long movie, but it was great.", 1),
    ("The cinematography was adequate, the sound was passable, and the acting was fine", 1),
    ("Bad start but excellent ending saves the film", 1),
    ("I expected this to be good, but it turned out to be great", 1),
    ("The prequel was terrible but this movie is actually good", 1),
    ("The actor's success streak broke with this film", 0),
    ("The story made up for the poor acting and weak plot", 1),
]

---

## 🔍 Observations & Insights

### 🟢 Simple Sentences

Example: "This is a good movie"  
Prediction: NB=0, LR=1, BERT=1  

Insight:  
Naive Bayes relies on token likelihoods; common words like "good" are not always strong indicators of positive sentiment.

---

### 🟢 Strong Polarity Words

Examples:  
- "Amazing acting and great story"  
- "Terrible plot and boring film"  

Observation: All models classify correctly  

Insight:  
Strong sentiment words make classification easy even for simple models.

---

### 🟡 Repetition Effect (NB Failure)

Example:  
"It was a long, long, long, long, long movie, but it was great."  
Prediction: NB=0, LR=1, BERT=1  

Insight:  
Naive Bayes amplifies repeated tokens, leading to incorrect predictions.

---

### 🟡 Neutral / Weak Sentiment

Example:  
"The cinematography was adequate, the sound was passable, and the acting was fine"  
Prediction: NB=0, LR=0, BERT=1  

Insight:  
Classical models interpret neutral words as negative due to dataset bias.

---

### 🔴 Contrast & Structure (BERT Advantage)

Examples:  

"Bad start but excellent ending saves the film"  
Prediction: NB=1, LR=0, BERT=1  

"I expected this to be good, but it turned out to be great"  
Prediction: NB=0, LR=1, BERT=1  

Insight:  
Classical models ignore structure (like "but"), while transformers capture contrast and correctly prioritize the second clause.

---

### ⚫ Hard Cases (All Models Struggle)

Example 1:  
"The prequel was terrible but this movie is actually good"  
Prediction: NB=0, LR=0, BERT=0  

Example 2:  
"The actor's success streak broke with this film"  
Prediction: NB=1, LR=1, BERT=1  

Example 3:  
"The story made up for the poor acting and weak plot"  
Prediction: NB=0, LR=0, BERT=0  

Insight:  
All models struggle with:
- implicit sentiment  
- compositional meaning  
- deeper reasoning  

---

## 🎯 Key Takeaways

### Naive Bayes
- Works well with strong polarity words  
- Fails with:
  - repeated tokens  
  - neutral sentiment  
  - contextual understanding  

---

### Logistic Regression
- Best classical model overall  
- Handles feature weighting better than NB  
- Still limited by lack of structure awareness  

---

### DistilBERT
- Best overall performance  
- Captures context and contrast  
- Still struggles with:
  - implicit meaning  
  - compositional reasoning  

---

## 🧠 Final Insight

Classical models rely on word-level signals, while transformers leverage context. However, all models remain limited in handling deeper semantic reasoning, highlighting the gap between pattern recognition and true language understanding.

---

## 🚀 Conclusion

This project demonstrates a progression:

- Naive Bayes → token-based reasoning  
- Logistic Regression → weighted feature reasoning  
- Transformers → contextual reasoning  

Even advanced models fail on deeper semantic tasks, indicating scope for further research.

---

## 📂 Project Structure

- transformer_vs_classical_nlp.ipynb  
- main.py  
- README.md  
- requirements.txt  