# ML_HACKATHON.ipynb - Complete Documentation

## 📚 Overview

This notebook implements a comprehensive Hangman-playing AI using Hidden Markov Models (HMM) and Reinforcement Learning (Q-Learning). The solution combines statistical analysis, machine learning, and game strategy to achieve optimal performance.

---

## 📊 Dataset Connections

### Primary Datasets:

1. **Data/corpus.txt** - Training corpus (~50,000 words)

   - Used for: HMM training, letter frequency analysis, Q-learning training
   - Purpose: Learn English word patterns, letter distributions, positional probabilities

2. **Data/test.txt** - Test set for evaluation
   - Used for: Final model evaluation, performance metrics
   - Purpose: Measure model generalization on unseen words

---

## 🔍 Cell-by-Cell Structure

### **CELL 1: Library Imports and Environment Setup**

- **Purpose**: Import all required Python libraries and configure environment
- **Libraries**: numpy, pandas, matplotlib, seaborn, collections, pickle, scipy, tqdm
- **Configuration**: Random seeds (42), plotting style, warning suppression
- **Dataset**: None (setup only)

---

### **CELL 2: Dataset Loading and Validation**

- **Purpose**: Load and validate training corpus and test set
- **Dataset Used**:
  - `Data/corpus.txt` → corpus_words
  - `Data/test.txt` → test_words
- **Validation**: File existence, non-empty checks, character validation
- **Output**: corpus_words, test_words (Python lists)

---

### **CELL 3: EDA - Word Length Distribution**

- **Purpose**: Analyze word length patterns for model design
- **Dataset Used**: corpus_words, test_words
- **Analysis**:
  - Statistical summary (mean, median, std, percentiles)
  - Histogram visualizations (corpus vs test)
  - Kolmogorov-Smirnov test for distribution similarity
- **Key Insight**: Length-specific models recommended due to high variance
- **Visualizations**: 4 plots (corpus hist, test hist, comparison bar, box plot)

---

### **CELL 4: EDA - Letter Frequency Analysis**

- **Purpose**: Identify most/least common letters for guessing strategy
- **Dataset Used**: All letters from corpus_words and test_words
- **Analysis**:
  - Overall letter frequency counting
  - Percentage normalization
  - Corpus-test correlation calculation
- **Key Insight**: Top 5 letters (e, t, a, o, i) should be guessed first
- **Visualizations**: 4 plots (alphabetical, sorted, comparative)

---

### **CELL 5: EDA - Positional Letter Analysis**

- **Purpose**: Analyze letter distribution at word positions (start/middle/end)
- **Dataset Used**: corpus_words, test_words (positional extraction)
- **Analysis**:
  - First letter frequency (position 0)
  - Middle letter frequency (positions 1 to n-2)
  - Last letter frequency (position n-1)
- **Key Insight**: Strong positional bias (s common at start, e at end)
- **Importance**: Informs HMM position-specific emission probabilities
- **Visualizations**: 6 plots (3 positions × 2 datasets)

---

### **CELL 6: EDA - Vowel vs Consonant Analysis**

- **Purpose**: Analyze vowel-consonant patterns and ratios
- **Dataset Used**: corpus_words, test_words
- **Analysis**:
  - Total vowel/consonant counts
  - Vowel ratio per word (vowels / word_length)
  - Distribution comparisons
- **Key Insight**: Vowels make up ~40% of letters, critical for early guesses
- **Visualizations**: 4 plots (vowel counts, consonant counts, ratios, scatter)

---

### **CELL 7: EDA - Unique Letter Analysis**

- **Purpose**: Analyze letter uniqueness and repetition patterns
- **Dataset Used**: corpus_words, test_words
- **Analysis**:
  - Unique letters per word
  - Uniqueness ratio (unique / length)
  - Words with repeated letters
- **Key Insight**: ~X% of words have repeated letters (harder to solve)
- **Visualizations**: 4 plots (unique counts, ratios, scatter plots)

---

### **CELL 8: EDA - N-gram and Pattern Analysis**

- **Purpose**: Extract bigram/trigram patterns for context-aware guessing
- **Dataset Used**: corpus_words, test_words
- **Analysis**:
  - Bigram frequency (2-letter sequences)
  - Trigram frequency (3-letter sequences)
  - Vowel-consonant pattern analysis
- **Key Insight**: Common bigrams (th, er, on) and trigrams inform HMM context
- **Importance**: Used in HMM for contextual letter prediction
- **Visualizations**: 4 plots (top bigrams, trigrams, comparison, patterns)

---

### **CELL 9: EDA - Word Complexity and Difficulty Analysis**

- **Purpose**: Calculate difficulty scores based on multiple factors
- **Dataset Used**: corpus_words, test_words
- **Analysis**:
  - Difficulty scoring function (length, repeats, rare letters, vowel ratio)
  - Distribution by difficulty category
  - Feature correlation matrix
- **Key Insight**: Identifies hardest words, guides RL reward shaping
- **Visualizations**: 4 plots (difficulty dist, length vs difficulty, categories, correlation)

---

### **CELL 10: EDA Summary and Key Insights**

- **Purpose**: Consolidate all EDA findings and save results
- **Dataset Used**: All previous EDA results
- **Output**:
  - Comprehensive insights summary
  - Key takeaways for model design
  - Saved to: `eda_results.pkl`
- **Key Recommendations**:
  - Start with high-frequency letters
  - Use HMM with positional encoding
  - Reward vowel discovery in RL
  - Apply EDA-based heuristics

---

### **CELL 11: Hidden Markov Model - Enhanced Design**

- **Purpose**: Implement EnhancedHangmanHMM class with EDA insights
- **Dataset Used**: corpus_words (for training)
- **Architecture**:
  - Hidden States: Letter positions in words
  - Emissions: Actual letters at positions
  - Length-specific models (separate HMM per word length)
  - Bigram/trigram context integration
- **Training Output**:
  - Position-specific probability distributions
  - Bigram/trigram probabilities
  - Global letter frequencies
- **Key Methods**:
  - `train(words)`: Train on corpus
  - `get_letter_probabilities(masked_word, guessed_letters)`: Predict next letter
- **Dataset Connection**: Trained on corpus_words from Data/corpus.txt

---

### **CELL 12: Test Enhanced HMM Performance**

- **Purpose**: Validate HMM predictions on sample words
- **Dataset Used**: Sample test words (manual examples)
- **Output**: Top 10 predicted letters for partially masked words
- **Validation**: Checks if correct letters are in top predictions

---

### **CELL 13: Hangman Environment**

- **Purpose**: Implement RL environment for agent training
- **Dataset Used**: corpus_words (for game word selection)
- **Class**: HangmanEnvironment
  - `reset()`: Start new game
  - `step(letter)`: Take action, return reward
  - `get_state()`: Current game state
  - `get_available_actions()`: Unguessed letters
- **Reward Function**:
  - +10 per letter revealed
  - +100 for winning
  - -5 for wrong guess
  - -10 for repeated guess
  - -50 for losing

---

### **CELL 14: Enhanced RL Agent with EDA Insights**

- **Purpose**: Implement Q-Learning agent with HMM+EDA hybrid strategy
- **Dataset Used**: Uses HMM trained on corpus_words
- **Class**: EnhancedHangmanAgent
  - Q-table for state-action values
  - Epsilon-greedy exploration
  - Hybrid decision making (60% Q + 30% HMM + 10% heuristics)
- **State Representation**: (word_pattern, length, lives_left, num_guessed)
- **Action Space**: 26 letters (filtered by guessed)
- **Exploration**: HMM-guided (not random)

---

### **CELL 15: Training Loop with Progress Tracking**

- **Purpose**: Train RL agent for 10,000 episodes
- **Dataset Used**: corpus_words (training environment)
- **Training Configuration**:
  - Episodes: 10,000
  - Print progress every 1,000 episodes
  - Track rewards, success rate, wrong guesses, epsilon
- **Output**: training_history dictionary with all metrics
- **Epsilon Decay**: 1.0 → ~0.01 over training

---

### **CELL 16: Comprehensive Training Visualization**

- **Purpose**: Visualize training progress and learning curves
- **Dataset Used**: training_history from previous cell
- **Visualizations** (6 plots):
  1. Rewards over time (smoothed)
  2. Success rate (rolling 100 episodes)
  3. Wrong guesses over time
  4. Episode lengths
  5. Epsilon decay curve
  6. Learning phases bar chart
- **Key Metrics**: Final success rate, avg reward, Q-table size

---

### **CELL 17: Evaluation Function**

- **Purpose**: Define comprehensive evaluation framework
- **Function**: `evaluate_agent(agent, test_words, ...)`
- **Metrics Calculated**:
  - Games played, won, lost
  - Success rate
  - Avg wrong/repeated guesses
  - Final score calculation
- **Returns**: Detailed results dictionary with game-by-game data

---

### **CELL 18: Run Complete Evaluation**

- **Purpose**: Evaluate trained agent on test set
- **Dataset Used**: test_words from Data/test.txt
- **Output**:
  - Complete evaluation metrics
  - Final score calculation
  - Detailed performance summary
- **Score Formula**:
  (success_rate × games) - (wrong_guesses × 5) - (repeated_guesses × 2)

---

### **CELL 19: Detailed Performance Analysis Visualizations**

- **Purpose**: Deep dive into evaluation results
- **Dataset Used**: eval_results from previous cell
- **Visualizations** (6 plots):
  1. Win/Loss pie chart
  2. Wrong guesses distribution
  3. Repeated guesses distribution
  4. Success rate by word length
  5. Wrong guesses by word length
  6. Score components breakdown
- **Insights**: Performance varies by word length, difficulty

---

### **CELL 20: Failure Analysis**

- **Purpose**: Identify and analyze failed games
- **Dataset Used**: eval_results game_details
- **Analysis**:
  - Top 20 most difficult words
  - Words with repeated guesses
  - Perfect games (0 wrong guesses)
  - Common patterns in failures
- **Output**: Detailed failure report for model improvement

---

### **CELL 21: Save All Models and Results**

- **Purpose**: Persist models and results to disk
- **Files Generated**:
  - `enhanced_hmm_model.pkl`: Trained HMM
  - `q_table.pkl`: Q-Learning table
  - `enhanced_agent.pkl`: Complete agent
  - `comprehensive_results.pkl`: All results
  - `detailed_evaluation_results.csv`: Per-game data
  - `FINAL_SUMMARY_REPORT.txt`: Text summary
- **Dataset Connection**: Saves models trained on corpus_words, evaluated on test_words

---

### **CELL 22: Baseline Comparison Analysis**

- **Purpose**: Compare enhanced agent vs simple baseline
- **Baseline**: Frequency-only agent (no RL, no HMM)
- **Dataset Used**: First 500 words from test_words
- **Comparison Metrics**:
  - Success rate
  - Avg wrong guesses
  - Final score
- **Visualizations**: Side-by-side performance comparison
- **Output**: Improvement percentage over baseline

---

### **CELL 23: Interactive Demo System**

- **Purpose**: Demonstrate agent playing live games
- **Function**: `play_interactive_demo(agent, word, ...)`
- **Features**:
  - Step-by-step gameplay
  - HMM predictions display
  - Reward tracking
  - Visual progress (hearts for lives)
- **Dataset Used**: Sample words from test_words
- **Output**: Interactive game playthrough with commentary

---

### **CELL 24: Analysis Report Data Export**

- **Purpose**: Export comprehensive analysis for documentation
- **Output Files**:
  - `analysis_report_data.pkl`: Structured analysis data
  - `ANALYSIS_REPORT.txt`: Formatted text report
- **Contents**:
  - Key observations (training, EDA, challenges, successes)
  - HMM design choices
  - RL state/action/reward design
  - Exploration vs exploitation strategy
  - Future improvements
  - Final metrics summary

---

### **CELL 25: Final Summary and Next Steps**

- **Purpose**: Comprehensive completion checklist
- **Output**: Summary of all implemented components
- **Checklist**:
  - ✅ Extensive EDA (Cells 3-10)
  - ✅ HMM implementation (Cells 11-12)
  - ✅ RL training (Cells 13-16)
  - ✅ Evaluation (Cells 17-21)
  - ✅ Additional features (Cells 22-24)
- **Files Generated**: List of all saved files
- **Preparation Guide**: Viva, demo, submission readiness

---

## 🎯 Key Model Components

### 1. Enhanced HMM (Cells 11-12)

**Trained on**: corpus_words from Data/corpus.txt

- **Architecture**: Length-specific position-based models
- **Inputs**: Masked word (e.g., "a\_\_le"), guessed letters
- **Output**: Probability distribution over unguessed letters
- **Integration**: Bigrams, trigrams, positional priors

### 2. Q-Learning Agent (Cells 14-15)

**Trained on**: corpus_words via HangmanEnvironment

- **State Space**: ~X,XXX unique states
- **Action Space**: 26 letters (dynamic filtering)
- **Learning**: 10,000 episodes with epsilon decay
- **Strategy**: Hybrid (Q-values + HMM + heuristics)

### 3. Evaluation Framework (Cells 17-20)

**Evaluated on**: test_words from Data/test.txt

- **Primary Metric**: Success rate (% games won)
- **Secondary**: Avg wrong guesses, repeated guesses
- **Final Score**: Composite metric with penalties

---

## 📈 Performance Summary

| Metric                      | Value  |
| --------------------------- | ------ |
| Training Episodes           | 10,000 |
| Final Training Success Rate | ~XX%   |
| Test Set Success Rate       | XX.X%  |
| Avg Wrong Guesses           | X.XX   |
| Avg Repeated Guesses        | X.XX   |
| Final Score                 | XXX.XX |
| Improvement over Baseline   | +XX.X% |

---

## 🔧 How to Use This Notebook

### **1. Load and Prepare Data**

Run Cells 1-2 to load datasets from `Data/` folder

### **2. Exploratory Analysis** (Optional but recommended)

Run Cells 3-10 to understand data characteristics

### **3. Train HMM**

Run Cell 11 to train HMM on corpus_words

### **4. Train RL Agent**

Run Cells 13-15 to train Q-Learning agent (takes ~10-20 minutes)

### **5. Evaluate**

Run Cells 17-18 to evaluate on test_words

### **6. Analyze Results**

Run Cells 19-25 for comprehensive analysis and reporting

---

## 📂 Generated Files Reference

| File                              | Description        | Source Cell |
| --------------------------------- | ------------------ | ----------- |
| `eda_results.pkl`                 | EDA analysis data  | Cell 10     |
| `enhanced_hmm_model.pkl`          | Trained HMM model  | Cell 21     |
| `q_table.pkl`                     | Q-Learning table   | Cell 21     |
| `enhanced_agent.pkl`              | Complete agent     | Cell 21     |
| `comprehensive_results.pkl`       | All results        | Cell 21     |
| `detailed_evaluation_results.csv` | Per-game details   | Cell 21     |
| `FINAL_SUMMARY_REPORT.txt`        | Summary report     | Cell 21     |
| `analysis_report_data.pkl`        | Analysis data      | Cell 24     |
| `ANALYSIS_REPORT.txt`             | Formatted analysis | Cell 24     |

---

## 🎓 Key Concepts Explained

### **Hidden Markov Model (HMM)**

- **Hidden States**: Positions in word (0, 1, 2, ..., n-1)
- **Observations**: Letters appearing at positions
- **Purpose**: Learn P(letter | position, word_length, context)
- **Training Data**: corpus_words from Data/corpus.txt

### **Reinforcement Learning (Q-Learning)**

- **Environment**: Hangman game with 6 lives
- **State**: (word_pattern, length, lives, guesses_made)
- **Action**: Choose a letter from a-z
- **Reward**: Points for correct (+10), penalties for wrong (-5)
- **Training Data**: corpus_words via game simulation

### **Hybrid Strategy**

- **60% Q-Values**: Learned strategy from RL training
- **30% HMM**: Language model probabilities
- **10% Heuristics**: EDA-based rules (vowel priority, frequency)
- **Result**: Best of all three approaches

---

## 🚀 Future Improvements (from Cell 24)

1. **Deep Q-Network (DQN)**: Neural network instead of Q-table
2. **Character-level LSTM/Transformer**: Modern NLP approaches
3. **Monte Carlo Tree Search (MCTS)**: Lookahead planning
4. **Prioritized Experience Replay**: Learn from hard examples
5. **Ensemble Methods**: Combine multiple models
6. **Transfer Learning**: Pre-train on large corpora
7. **Adaptive Difficulty**: Different strategies per word type
8. **Multi-objective Optimization**: Balance speed, accuracy, efficiency

---

## 📞 Contact & Support

For questions about this implementation:

- Review cell comments for detailed explanations
- Check `ANALYSIS_REPORT.txt` for design rationale
- See `FINAL_SUMMARY_REPORT.txt` for performance metrics

---

**Last Updated**: Generated automatically by Cell 25
**Total Cells**: 25
**Total Lines**: 2,594
**Datasets Used**: Data/corpus.txt, Data/test.txt
