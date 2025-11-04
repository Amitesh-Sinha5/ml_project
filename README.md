# Hangman AI - ML Hackathon Project

## 🎯 Project Overview

This project implements an intelligent Hangman-playing agent using **Hidden Markov Models (HMM)** and **Reinforcement Learning (Q-Learning)**. The hybrid approach combines statistical language modeling with learning-based strategy optimization to achieve superior performance.

---

## 📁 Project Structure

```
MLPROject/
├── README.md                           # This file
├── 054_024_026_039.ipynb              # Main notebook (25 cells, fully executed)
│
├── Data/                               # Training and test datasets
│   ├── corpus.txt                      # Training corpus (~50,000 words)
│   └── test.txt                        # Test set for evaluation
│
├── Documentation/                      # Comprehensive guides
│   ├── QUICK_START_GUIDE.md           # How to run the notebook
│   ├── NOTEBOOK_DOCUMENTATION.md      # Cell-by-cell explanations
│   ├── DATASET_CONNECTION_MAP.md      # Data flow visualization
│   └── COMPLETE_IMPLEMENTATION_GUIDE.md # Complete overview
│
├── Models/                             # Trained models (generated)
│   ├── enhanced_hmm_model.pkl         # Trained HMM
│   ├── enhanced_agent.pkl             # Complete RL agent
│   └── q_table.pkl                    # Q-Learning table
│
├── Results/                            # Evaluation results (generated)
│   ├── comprehensive_results.pkl      # All metrics
│   ├── detailed_evaluation_results.csv # Per-game details
│   ├── eda_results.pkl                # EDA analysis
│   ├── analysis_report_data.pkl       # Analysis data
│   ├── FINAL_SUMMARY_REPORT.txt       # Summary report
│   └── ANALYSIS_REPORT.txt            # Detailed analysis
│
└── notebooks/                          # Additional notebooks (if any)
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- Jupyter Notebook or VS Code with Jupyter extension
- Required libraries: numpy, pandas, matplotlib, seaborn, scipy, tqdm

### Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd MLPROject
   ```

2. **Install dependencies**

   ```bash
   pip install numpy pandas matplotlib seaborn scipy tqdm
   ```

3. **Verify data files exist**
   ```bash
   ls Data/corpus.txt Data/test.txt
   ```

### Running the Notebook

**Option 1: Run All Cells** (First-time users)

```bash
# Open 054_024_026_039.ipynb in Jupyter/VS Code
# Click "Run All" or Ctrl+Shift+Enter
# Wait ~15-30 minutes for training
```

**Option 2: Step-by-Step Execution**

1. **Cells 1-2**: Setup and data loading (< 1 min)
2. **Cells 3-10**: Exploratory Data Analysis (~ 5 min)
3. **Cells 11-12**: Train HMM (~ 2 min)
4. **Cells 13-16**: Train RL agent (~ 15-20 min)
5. **Cells 17-21**: Evaluation and save models (~ 5 min)
6. **Cells 22-25**: Baseline comparison and reports (~ 3 min)

---

## 📊 Results Summary

### Performance Metrics

- **Success Rate**: ~50-55% (games won)
- **Average Wrong Guesses**: ~2.5-3.0
- **Improvement over Baseline**: +10-15 percentage points
- **Training Episodes**: 10,000
- **Q-Table Size**: Varies (typically 5,000-15,000 states)

### Key Features

✅ **Comprehensive EDA**: 8 analysis cells with 20+ visualizations  
✅ **Hybrid Approach**: Combines HMM (30%) + Q-Learning (60%) + Heuristics (10%)  
✅ **Length-Specific Models**: Adapts to word length patterns  
✅ **Context-Aware**: Uses bigrams/trigrams for better prediction  
✅ **Production-Ready**: Saves models, generates reports, creates CSVs

---

## 🧠 Model Architecture

### 1. Hidden Markov Model (HMM)

- **Purpose**: Learn letter-position probability distributions
- **Training Data**: `Data/corpus.txt` (~50,000 words)
- **Architecture**:
  - Hidden States: Letter positions (0, 1, 2, ..., n-1)
  - Emissions: Actual letters at positions
  - Length-specific models for different word lengths
  - Bigram/trigram context integration

### 2. Reinforcement Learning (Q-Learning)

- **Purpose**: Learn optimal guessing strategy
- **Training**: 10,000 game simulations
- **State**: (word_pattern, length, lives_left, guesses_made)
- **Action**: Choose letter from a-z
- **Reward Function**:
  - +10 per letter revealed
  - +100 for winning
  - -5 for wrong guess
  - -10 for repeated guess
  - -50 for losing

### 3. Hybrid Decision Making

- **Final Decision** = 60% Q-values + 30% HMM + 10% EDA heuristics
- **Exploration**: HMM-guided epsilon-greedy (not random)
- **Exploitation**: Weighted combination of all three signals

---

## 📈 Key Insights from EDA

1. **Letter Frequency**: Top 5 letters (e, t, a, o, i) should be guessed first
2. **Position Matters**: 's' common at start, 'e' common at end
3. **Vowel Importance**: ~40% of all letters, critical for early guesses
4. **N-gram Patterns**: Common bigrams (th, er, on) inform context
5. **Difficulty**: Words with repeated letters significantly harder

---

## 📂 Generated Files

### After Running the Notebook:

| File                              | Purpose                                   |
| --------------------------------- | ----------------------------------------- |
| `enhanced_hmm_model.pkl`          | Trained HMM with position-specific models |
| `enhanced_agent.pkl`              | Complete RL agent with Q-table            |
| `q_table.pkl`                     | Q-Learning state-action values            |
| `comprehensive_results.pkl`       | All evaluation metrics                    |
| `detailed_evaluation_results.csv` | Per-game detailed results                 |
| `eda_results.pkl`                 | EDA analysis data                         |
| `analysis_report_data.pkl`        | Structured analysis data                  |
| `FINAL_SUMMARY_REPORT.txt`        | Summary report with metrics               |
| `ANALYSIS_REPORT.txt`             | Detailed design rationale                 |

---

## 📚 Documentation

### Quick References

1. **QUICK_START_GUIDE.md** - Step-by-step execution instructions
2. **NOTEBOOK_DOCUMENTATION.md** - Cell-by-cell breakdown (all 25 cells)
3. **DATASET_CONNECTION_MAP.md** - Data flow and transformations
4. **COMPLETE_IMPLEMENTATION_GUIDE.md** - Complete overview

### Key Concepts

- **Cell 1-2**: Setup and data loading
- **Cell 3-10**: Comprehensive EDA
- **Cell 11-12**: HMM training and testing
- **Cell 13-14**: RL environment and agent
- **Cell 15-16**: Training with visualization
- **Cell 17-21**: Evaluation and model saving
- **Cell 22-25**: Baseline comparison and reports

---

## 🎓 Technical Details

### Dataset Connections

- **Data/corpus.txt** → Training (HMM, RL, EDA)
- **Data/test.txt** → Evaluation (final metrics)

### Training Process

1. Load datasets (Cell 2)
2. Analyze patterns via EDA (Cells 3-10)
3. Train HMM on corpus_words (Cell 11)
4. Create RL environment (Cell 13)
5. Train Q-Learning agent for 10,000 episodes (Cell 15)
6. Evaluate on test_words (Cell 18)
7. Save models and generate reports (Cell 21)

### Hyperparameters

- **Learning Rate (α)**: 0.1
- **Discount Factor (γ)**: 0.95
- **Initial Epsilon (ε)**: 1.0
- **Epsilon Decay**: 0.995
- **Min Epsilon**: 0.01
- **Training Episodes**: 10,000

---

## 🔧 Troubleshooting

### Common Issues

**Issue**: `FileNotFoundError: Data/corpus.txt`  
**Solution**: Ensure `Data/` folder exists with corpus.txt and test.txt

**Issue**: Training too slow  
**Solution**: Reduce training episodes in Cell 15 (try 5,000 instead of 10,000)

**Issue**: Low success rate (<40%)  
**Solution**:

- Verify datasets are properly formatted (lowercase, one word per line)
- Check HMM trained successfully (Cell 11 output)
- Train for more episodes

**Issue**: Memory error  
**Solution**: Reduce corpus size or clear variables between runs

---

## 🎯 Future Improvements

1. **Deep Q-Network (DQN)**: Replace Q-table with neural network
2. **LSTM/Transformer**: Use modern NLP models for character prediction
3. **Monte Carlo Tree Search (MCTS)**: Add lookahead planning
4. **Prioritized Experience Replay**: Learn more from difficult examples
5. **Ensemble Methods**: Combine multiple models
6. **Transfer Learning**: Pre-train on large corpora (Wikipedia)

---

## 👥 Contributors

- Project developed for ML Hackathon
- Repository: SE-LAB5-STATIC-CODE-ANALYSIS
- Owner: Amitesh-Sinha5

---

## 📄 License

This project is part of an academic assignment/hackathon.

---

## 📞 Support

For questions:

1. Check documentation files in Documentation/ directory
2. Review cell comments in 054_024_026_039.ipynb
3. Read Results/FINAL_SUMMARY_REPORT.txt for metrics
4. See Results/ANALYSIS_REPORT.txt for design rationale

---

## ✨ Acknowledgments

- Dataset: English word corpus
- Libraries: NumPy, Pandas, Matplotlib, Seaborn, SciPy
- Approach: Hybrid HMM + RL + EDA heuristics

---

**Last Updated**: November 4, 2025  
**Status**: ✅ Fully Implemented and Tested  
**Notebook Status**: All 25 cells executed successfully
