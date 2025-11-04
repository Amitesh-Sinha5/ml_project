# Dataset Connection Map - ML_HACKATHON.ipynb

## 🗺️ Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    PRIMARY DATASETS                              │
├─────────────────────────────────────────────────────────────────┤
│  Data/corpus.txt  (~50,000 words)  →  corpus_words (Python list)│
│  Data/test.txt    (test set)       →  test_words   (Python list)│
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    CELL 2: Data Loading                          │
│  Input:  Data/corpus.txt, Data/test.txt (text files)            │
│  Output: corpus_words, test_words (lists)                       │
│  Format: ['apple', 'banana', 'cherry', ...]                     │
└─────────────────────────────────────────────────────────────────┘
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
          ▼                                   ▼
┌─────────────────────┐           ┌─────────────────────┐
│   CELLS 3-10: EDA   │           │  CELLS 11-12: HMM   │
│   (Exploratory      │           │  (Model Training)   │
│   Data Analysis)    │           │                     │
├─────────────────────┤           ├─────────────────────┤
│ Uses: corpus_words  │           │ Uses: corpus_words  │
│       test_words    │           │                     │
│                     │           │ Trains:             │
│ Outputs:            │           │ - Position probs    │
│ - Visualizations    │           │ - Bigram probs      │
│ - Statistics        │           │ - Trigram probs     │
│ - eda_results.pkl   │           │                     │
│                     │           │ Outputs:            │
│ Insights:           │           │ - hmm (object)      │
│ - Letter freq       │           │ - Model metrics     │
│ - Position patterns │           │                     │
│ - N-grams           │           └─────────────────────┘
│ - Difficulty        │                     │
└─────────────────────┘                     │
          │                                 │
          │                                 ▼
          │                   ┌─────────────────────────┐
          │                   │  CELLS 13-14: RL Setup  │
          │                   │  (Agent + Environment)  │
          │                   ├─────────────────────────┤
          │                   │ Uses: corpus_words      │
          │                   │       hmm (from Cell 11)│
          │                   │                         │
          │                   │ Creates:                │
          │                   │ - HangmanEnvironment    │
          │                   │ - EnhancedHangmanAgent  │
          │                   │                         │
          │                   │ Agent uses:             │
          │                   │ - Q-table (initially ∅) │
          │                   │ - HMM probabilities     │
          │                   │ - EDA heuristics        │
          │                   └─────────────────────────┘
          │                             │
          │                             ▼
          │                   ┌─────────────────────────┐
          │                   │  CELLS 15-16: Training  │
          │                   │  (Q-Learning)           │
          │                   ├─────────────────────────┤
          │                   │ Uses: corpus_words      │
          │                   │       agent (Cell 14)   │
          │                   │       env (Cell 13)     │
          │                   │                         │
          │                   │ Process:                │
          │                   │ 10,000 episodes         │
          │                   │ Each episode:           │
          │                   │ 1. Sample word from     │
          │                   │    corpus_words         │
          │                   │ 2. Agent plays game     │
          │                   │ 3. Update Q-table       │
          │                   │                         │
          │                   │ Outputs:                │
          │                   │ - training_history      │
          │                   │ - Trained Q-table       │
          │                   │ - Visualizations        │
          │                   └─────────────────────────┘
          │                             │
          │                             ▼
          └─────────────────────────────┼─────────────────────────┐
                                        │                         │
                                        ▼                         ▼
                          ┌─────────────────────────┐  ┌─────────────────────┐
                          │  CELLS 17-18: Evaluate  │  │  CELL 22: Baseline  │
                          │  (Test Set Performance) │  │  (Comparison)       │
                          ├─────────────────────────┤  ├─────────────────────┤
                          │ Uses: test_words        │  │ Uses: test_words    │
                          │       trained agent     │  │       corpus_words  │
                          │                         │  │                     │
                          │ Process:                │  │ Creates:            │
                          │ For each test word:     │  │ - Baseline agent    │
                          │ 1. Agent plays game     │  │   (frequency only)  │
                          │ 2. Record metrics       │  │                     │
                          │                         │  │ Compares:           │
                          │ Outputs:                │  │ - Enhanced vs       │
                          │ - eval_results          │  │   Baseline          │
                          │ - Success rate          │  │ - Performance gain  │
                          │ - Final score           │  └─────────────────────┘
                          └─────────────────────────┘
                                        │
                                        ▼
                          ┌─────────────────────────┐
                          │  CELLS 19-21: Analysis  │
                          │  (Detailed Reporting)   │
                          ├─────────────────────────┤
                          │ Uses: eval_results      │
                          │       training_history  │
                          │       all EDA data      │
                          │                         │
                          │ Generates:              │
                          │ - 6 visualization plots │
                          │ - Failure analysis      │
                          │ - All .pkl files        │
                          │ - Reports (.txt)        │
                          │ - CSV results           │
                          └─────────────────────────┘
```

---

## 📊 Dataset Usage Summary

### **Data/corpus.txt** → **corpus_words**

| Cell | Purpose        | How Used                         |
| ---- | -------------- | -------------------------------- |
| 2    | Load data      | Read file, create list           |
| 3-10 | EDA            | Analyze patterns, statistics     |
| 11   | HMM Training   | Train position-specific models   |
| 13   | RL Environment | Word pool for game simulation    |
| 15   | RL Training    | Sample words for 10,000 episodes |
| 22   | Baseline       | Train baseline frequency agent   |

**Total Uses**: 7 major components
**Purpose**: Model training and learning

---

### **Data/test.txt** → **test_words**

| Cell  | Purpose    | How Used                     |
| ----- | ---------- | ---------------------------- |
| 2     | Load data  | Read file, create list       |
| 3-10  | EDA        | Compare with corpus stats    |
| 17-18 | Evaluation | Final performance testing    |
| 19-20 | Analysis   | Detailed result analysis     |
| 22    | Baseline   | Compare baseline performance |
| 23    | Demo       | Interactive demonstrations   |

**Total Uses**: 6 major components
**Purpose**: Model evaluation and validation

---

## 🔄 Data Transformations

### **Raw Text Files → Python Lists**

```
Data/corpus.txt:
apple
banana
cherry

↓ (Cell 2)

corpus_words = ['apple', 'banana', 'cherry']
```

### **Words → Length Statistics**

```
corpus_words = ['apple', 'banana', 'cherry']

↓ (Cell 3)

corpus_lengths = [5, 6, 6]
Mean: 5.67, Median: 6
```

### **Words → Letter Frequencies**

```
corpus_words = ['apple', 'banana', 'cherry']

↓ (Cell 4)

corpus_letter_freq = Counter({
    'a': 4, 'p': 2, 'e': 2, 'l': 1,
    'b': 1, 'n': 2, 'c': 1, 'h': 1,
    'r': 2, 'y': 1
})
```

### **Words → HMM Position Probabilities**

```
corpus_words = ['apple', 'banana', 'cherry']

↓ (Cell 11)

hmm.models[5] = {  # 5-letter words
    'position_probs': {
        0: {'a': 0.4, 'c': 0.2, ...},  # First position
        1: {'p': 0.3, 'h': 0.2, ...},  # Second position
        ...
    }
}
```

### **Words → Q-Table States**

```
Word: "apple" with guesses ['a', 'p']
Revealed: "app__"

↓ (Cell 14)

State: ("011__", length=5, lives=6, guessed=2)
Q-table[state] = {'l': 0.8, 'e': 0.9, ...}
```

---

## 📈 Data Dependencies Graph

```
corpus.txt ──┬──> corpus_words ──┬──> EDA (Cells 3-10)
             │                    │
test.txt ────┴──> test_words ────┤
                                 │
                                 ├──> HMM Training (Cell 11)
                                 │         │
                                 │         ├──> Position Probs
                                 │         ├──> Bigram Probs
                                 │         └──> Trigram Probs
                                 │
                                 ├──> RL Environment (Cell 13)
                                 │         │
                                 └─────────┴──> RL Training (Cell 15)
                                               │
                                               ├──> Q-Table
                                               └──> Trained Agent
                                                         │
                  test_words ─────────────────────────────┴──> Evaluation
                                                               (Cells 17-18)
```

---

## 🎯 Critical Data Checkpoints

### **Checkpoint 1: Data Loading (Cell 2)**

```python
assert len(corpus_words) > 0, "Corpus not loaded!"
assert len(test_words) > 0, "Test set not loaded!"
print(f"✓ Loaded {len(corpus_words)} training words")
print(f"✓ Loaded {len(test_words)} test words")
```

### **Checkpoint 2: HMM Training (Cell 11)**

```python
assert len(hmm.models) > 0, "HMM not trained!"
assert len(hmm.bigram_probs) > 0, "Bigrams missing!"
print(f"✓ HMM trained with {len(hmm.models)} length-specific models")
```

### **Checkpoint 3: RL Training (Cell 15)**

```python
assert len(agent.q_table) > 0, "Q-table empty!"
assert len(training_history['rewards']) == 10000, "Training incomplete!"
print(f"✓ Agent trained for 10,000 episodes")
print(f"✓ Q-table size: {len(agent.q_table)} states")
```

### **Checkpoint 4: Evaluation (Cell 18)**

```python
assert eval_results['games_played'] == len(test_words), "Evaluation incomplete!"
print(f"✓ Evaluated on {eval_results['games_played']} test words")
print(f"✓ Success rate: {eval_results['success_rate']*100:.2f}%")
```

---

## 🗂️ Intermediate Data Products

| Variable             | Created | Type                 | Description                    |
| -------------------- | ------- | -------------------- | ------------------------------ |
| `corpus_words`       | Cell 2  | list                 | Training words from corpus.txt |
| `test_words`         | Cell 2  | list                 | Test words from test.txt       |
| `corpus_lengths`     | Cell 3  | list                 | Word lengths for EDA           |
| `corpus_letter_freq` | Cell 4  | Counter              | Letter frequency counts        |
| `hmm`                | Cell 11 | EnhancedHangmanHMM   | Trained HMM model              |
| `agent`              | Cell 14 | EnhancedHangmanAgent | RL agent                       |
| `training_history`   | Cell 15 | dict                 | Training metrics               |
| `eval_results`       | Cell 18 | dict                 | Evaluation results             |

---

## 💾 Saved Files (Dataset Derivatives)

| File                              | Source Dataset              | Contains       |
| --------------------------------- | --------------------------- | -------------- |
| `eda_results.pkl`                 | corpus_words, test_words    | EDA statistics |
| `enhanced_hmm_model.pkl`          | corpus_words                | Trained HMM    |
| `q_table.pkl`                     | corpus_words (via training) | Q-values       |
| `comprehensive_results.pkl`       | test_words (via eval)       | All metrics    |
| `detailed_evaluation_results.csv` | test_words                  | Per-game data  |

---

## 🔍 Dataset Quality Checks

### **Corpus.txt Requirements**:

- ✅ One word per line
- ✅ Lowercase letters only
- ✅ No special characters
- ✅ No empty lines
- ✅ Minimum 1,000 words (recommended: 10,000+)

### **Test.txt Requirements**:

- ✅ Same format as corpus.txt
- ✅ Different words from corpus (no overlap recommended)
- ✅ Minimum 100 words (recommended: 500+)
- ✅ Representative of corpus distribution

### **Validation (Cell 2)**:

```python
# Character validation
corpus_chars = set(''.join(corpus_words))
assert corpus_chars.issubset(set(string.ascii_lowercase)), "Invalid characters!"

# Size validation
assert len(corpus_words) >= 1000, "Corpus too small!"
assert len(test_words) >= 100, "Test set too small!"
```

---

## 📖 How to Trace Data Usage

### **Example: Letter 'e' through pipeline**

1. **Cell 2**: 'e' appears in corpus words

   ```
   corpus.txt: "apple\n..." → corpus_words = ['apple', ...]
   ```

2. **Cell 4**: 'e' counted in frequency analysis

   ```
   corpus_letter_freq['e'] = 12543 (count in corpus)
   ```

3. **Cell 11**: 'e' probabilities learned by HMM

   ```
   hmm.models[5]['position_probs'][4]['e'] = 0.25
   (25% chance 'e' at position 4 in 5-letter words)
   ```

4. **Cell 15**: Agent learns when to guess 'e'

   ```
   Q[state, 'e'] = 0.85 (high value for guessing 'e')
   ```

5. **Cell 18**: 'e' guessed correctly on test words
   ```
   eval_results shows 'e' in top-5 most successful guesses
   ```

---

## 🎓 Summary

**Primary Datasets**:

- `Data/corpus.txt` → Training (HMM + RL)
- `Data/test.txt` → Evaluation (unseen data)

**Data Flow**:

1. Load → 2. Analyze → 3. Train → 4. Evaluate → 5. Report

**Key Transformations**:

- Words → Statistics (EDA)
- Words → Probabilities (HMM)
- Words → Experiences (RL)
- Experiences → Q-values (Learning)
- Test words → Metrics (Evaluation)

**All data connections clearly documented in cells!**
