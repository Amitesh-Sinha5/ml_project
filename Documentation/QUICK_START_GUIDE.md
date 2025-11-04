# ML_HACKATHON.ipynb - Quick Start Guide

## 📋 Overview

This notebook implements a **Hangman-playing AI** using:

- **Hidden Markov Models (HMM)** for letter prediction
- **Q-Learning (Reinforcement Learning)** for strategy optimization
- **Hybrid approach** combining HMM + RL + EDA heuristics

---

## 🎯 What This Notebook Does

1. **Analyzes** word patterns in training data (EDA)
2. **Trains** HMM on letter-position probabilities
3. **Trains** RL agent to learn optimal guessing strategy
4. **Evaluates** performance on test set
5. **Generates** comprehensive reports and visualizations

---

## 📂 Dataset Requirements

Place these files in `Data/` folder:

```
Data/
├── corpus.txt    # Training corpus (~50,000 words)
└── test.txt      # Test set for evaluation
```

**Dataset Format**: One word per line, lowercase, no special characters

---

## 🚀 How to Run

### **Option 1: Run All Cells** (Recommended for first time)

1. Open notebook in Jupyter/VS Code
2. Click "Run All" or press `Ctrl+Shift+Enter`
3. Wait ~15-30 minutes for training to complete
4. Check generated files in project folder

### **Option 2: Run Step-by-Step**

#### **Step 1: Setup (Cells 1-2)**

```python
# Cell 1: Import libraries
# Cell 2: Load datasets from Data/corpus.txt and Data/test.txt
```

**Output**: `corpus_words`, `test_words` loaded

---

#### **Step 2: Exploratory Data Analysis (Cells 3-10)** _(Optional but insightful)_

```python
# Cell 3: Word length distribution
# Cell 4: Letter frequency analysis
# Cell 5: Positional letter patterns
# Cell 6: Vowel vs consonant analysis
# Cell 7: Unique letter patterns
# Cell 8: N-gram (bigram/trigram) analysis
# Cell 9: Word difficulty scoring
# Cell 10: EDA summary and insights
```

**Output**: Visualizations, statistics, `eda_results.pkl`

**Key Insights from EDA**:

- Top 5 letters to guess first: e, t, a, o, i
- Vowels critical (~40% of all letters)
- Position matters (s common at start, e at end)
- Words with repeated letters harder to solve

---

#### **Step 3: Train Hidden Markov Model (Cells 11-12)**

```python
# Cell 11: Define and train EnhancedHangmanHMM
hmm = EnhancedHangmanHMM()
hmm.train(corpus_words)  # Trains on Data/corpus.txt

# Cell 12: Test HMM predictions
```

**Output**: Trained HMM with length-specific models

**What HMM Does**:

- Learns P(letter | position, word_length)
- Integrates bigram/trigram context
- Provides probability distribution for next guess

**Training Time**: ~1-2 minutes

---

#### **Step 4: Create RL Environment (Cell 13)**

```python
# Cell 13: Define HangmanEnvironment class
# - Simulates Hangman game
# - Provides rewards for correct/wrong guesses
# - Tracks game state
```

**Output**: Environment ready for agent training

---

#### **Step 5: Train RL Agent (Cells 14-16)**

```python
# Cell 14: Define EnhancedHangmanAgent
agent = EnhancedHangmanAgent(hmm)  # Uses trained HMM

# Cell 15: Train agent for 10,000 episodes
training_history = train_agent(agent, training_env, num_episodes=10000)

# Cell 16: Visualize training progress
```

**Output**:

- Trained agent with Q-table
- `training_history` with metrics
- 6 training visualization plots

**Training Time**: ~10-20 minutes

**Training Progress**:

- Episode 0: Random guessing (~10% success)
- Episode 5000: Learning patterns (~35% success)
- Episode 10000: Converged (~45-55% success)

---

#### **Step 6: Evaluate on Test Set (Cells 17-18)**

```python
# Cell 17: Define evaluation function
# Cell 18: Run evaluation on test_words from Data/test.txt
eval_results = evaluate_agent(agent, test_words)
```

**Output**:

- Success rate, avg wrong/repeated guesses
- **Final Score** (main metric)
- Game-by-game details

**Evaluation Time**: ~2-5 minutes

---

#### **Step 7: Detailed Analysis (Cells 19-21)**

```python
# Cell 19: Performance visualizations (6 plots)
# Cell 20: Failure analysis (hardest words, patterns)
# Cell 21: Save all models and results
```

**Files Generated**:

- `enhanced_hmm_model.pkl`
- `enhanced_agent.pkl`
- `q_table.pkl`
- `comprehensive_results.pkl`
- `detailed_evaluation_results.csv`
- `FINAL_SUMMARY_REPORT.txt`

---

#### **Step 8: Comparison & Demos (Cells 22-25)**

```python
# Cell 22: Compare with baseline (frequency-only agent)
# Cell 23: Interactive demo (watch agent play)
# Cell 24: Export analysis report
# Cell 25: Final summary and checklist
```

**Output**:

- Baseline comparison charts
- Interactive game demonstrations
- `ANALYSIS_REPORT.txt`
- Completion checklist

---

## 📊 Key Metrics Explained

### **Success Rate**

- Percentage of games won (word guessed within 6 wrong guesses)
- **Target**: >50% for good performance
- **Excellent**: >60%

### **Average Wrong Guesses**

- Mean number of incorrect guesses per game
- **Target**: <3.0
- **Excellent**: <2.5

### **Final Score**

- Composite metric: `(success_rate × games) - (wrong × 5) - (repeated × 2)`
- **Higher is better**
- Balances success, efficiency, and accuracy

---

## 🎓 Understanding the Approach

### **1. Hidden Markov Model (HMM)**

**What it does**: Predicts letter probabilities based on word patterns

**Example**:

```
Masked word: "a__le"
Known: position 0='a', position 3='l', position 4='e'
Unknown: positions 1, 2

HMM prediction:
  p: 0.45  (high - "apple" is common)
  n: 0.15  (medium - "ankle" possible)
  b: 0.08  (low - "able" has 4 letters)
```

**How it's trained**:

1. Group corpus words by length
2. Count letters at each position
3. Calculate P(letter | position)
4. Add bigram/trigram context

---

### **2. Q-Learning Agent**

**What it does**: Learns optimal guessing strategy through trial and error

**State**: (word_pattern, length, lives_left, num_guessed)
**Action**: Choose a letter from a-z
**Reward**:

- +10 per letter revealed
- +100 for winning
- -5 for wrong guess
- -10 for repeated guess

**Learning**:

- Starts random (ε=1.0)
- Gradually exploits learned patterns (ε→0.01)
- Builds Q-table mapping states to best actions

---

### **3. Hybrid Decision Making**

**Final decision** = 60% Q-values + 30% HMM + 10% EDA heuristics

**Why hybrid?**

- Q-values: Captures learned game strategy
- HMM: Provides language model knowledge
- Heuristics: Adds domain expertise (vowel priority)

**Result**: Better than any single approach alone

---

## 🔧 Troubleshooting

### **Error: File not found "Data/corpus.txt"**

**Solution**: Create `Data/` folder and add corpus.txt, test.txt

### **Slow training**

**Solution**: Reduce `num_episodes` in Cell 15 (try 5000 instead of 10000)

### **Low success rate (<40%)**

**Solutions**:

- Train longer (increase episodes)
- Check if datasets are properly formatted
- Verify HMM trained successfully (Cell 11 output)

### **Memory error during training**

**Solution**:

- Reduce corpus size
- Clear old variables: `del training_history` between reruns

---

## 📈 Expected Results

### **Typical Performance** (after 10,000 episodes):

| Metric               | Expected Range     |
| -------------------- | ------------------ |
| Success Rate         | 45-55%             |
| Avg Wrong Guesses    | 2.5-3.5            |
| Avg Repeated Guesses | <0.1               |
| Final Score          | Varies by test set |
| Training Time        | 10-20 minutes      |
| Evaluation Time      | 2-5 minutes        |

### **Improvement over Baseline**:

- Baseline (frequency-only): ~35-40% success rate
- Enhanced Agent: ~45-55% success rate
- **Improvement**: +10-15 percentage points

---

## 📁 Generated Files Reference

| File                              | When Created | Purpose           |
| --------------------------------- | ------------ | ----------------- |
| `eda_results.pkl`                 | Cell 10      | EDA analysis data |
| `enhanced_hmm_model.pkl`          | Cell 21      | Trained HMM       |
| `q_table.pkl`                     | Cell 21      | Q-Learning table  |
| `enhanced_agent.pkl`              | Cell 21      | Complete agent    |
| `comprehensive_results.pkl`       | Cell 21      | All results       |
| `detailed_evaluation_results.csv` | Cell 21      | Per-game data     |
| `FINAL_SUMMARY_REPORT.txt`        | Cell 21      | Summary           |
| `ANALYSIS_REPORT.txt`             | Cell 24      | Analysis          |

---

## 🎤 Viva/Demo Preparation

### **Key Points to Explain**:

1. **EDA Insights**:

   - "We found that letter 'e' appears in 12% of all positions"
   - "Words starting with 's' are very common"
   - "Vowels make up 40% of letters - critical for early guesses"

2. **HMM Design**:

   - "We use separate models for each word length"
   - "Position-specific probabilities: 'e' likely at end, 's' at start"
   - "Bigram context: if previous letter is 't', 'h' is likely"

3. **RL Design**:

   - "State captures word pattern, not exact letters (generalization)"
   - "Reward function balances success (+100) and efficiency (-5 per wrong)"
   - "Epsilon-greedy: starts random, learns patterns, exploits knowledge"

4. **Hybrid Approach**:
   - "Combines strengths: RL learns strategy, HMM knows language"
   - "60-30-10 weighting found empirically to work best"
   - "Outperforms baseline by 10-15 percentage points"

---

## 🚀 Next Steps After Running

1. ✅ **Review Visualizations**: Check all plots for insights
2. ✅ **Read Reports**: Open `FINAL_SUMMARY_REPORT.txt` and `ANALYSIS_REPORT.txt`
3. ✅ **Test Interactive Demo**: Run Cell 23 to watch agent play
4. ✅ **Prepare Viva**: Practice explaining design choices
5. ✅ **Document Learnings**: Note what worked and what didn't

---

## 💡 Pro Tips

1. **First Run**: Execute all cells sequentially to generate all files
2. **Experiments**: Modify hyperparameters (learning rate, epsilon) in Cell 14
3. **Debugging**: Use Cell 23 interactive demo to see agent reasoning
4. **Performance**: Check Cell 20 for failure patterns to improve
5. **Documentation**: Refer to `NOTEBOOK_DOCUMENTATION.md` for detailed cell explanations

---

## 📞 Need Help?

**Check these in order**:

1. Cell output messages (detailed error info)
2. `NOTEBOOK_DOCUMENTATION.md` (comprehensive cell guide)
3. Comments in cells (inline explanations)
4. Generated reports (analysis and insights)

**Common Questions**:

**Q**: How long should training take?
**A**: 10-20 minutes for 10,000 episodes on modern hardware

**Q**: What's a good success rate?
**A**: >50% is good, >60% is excellent for this problem

**Q**: Can I use my own word lists?
**A**: Yes! Just format as one word per line in `Data/corpus.txt` and `Data/test.txt`

**Q**: How do I improve performance?
**A**:

- Train longer (more episodes)
- Tune hyperparameters (alpha, gamma in Cell 14)
- Add more EDA heuristics
- Experiment with reward function

---

## ✨ Summary

This notebook provides a **complete end-to-end solution** for Hangman AI:

- ✅ Comprehensive EDA
- ✅ Probabilistic model (HMM)
- ✅ Learning-based strategy (Q-Learning)
- ✅ Hybrid decision making
- ✅ Extensive evaluation
- ✅ Detailed documentation

**Ready to run!** Just execute all cells and wait for results. Good luck! 🎉
