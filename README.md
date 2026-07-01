# ai201-project3-takemeter
A fine-tuned text classifier evaluating discourse quality on r/nba. Compares a custom DistilBERT model against a zero-shot Llama-3.3 baseline to categorize sports discourse into analysis, hot takes, and reactionary vents.

# TakeMeter — Final Evaluation Report

## 1. Evaluation Report

### Overall Performance Summary
*   **Zero-shot baseline (Groq Llama-3.3-70b):** 77.78% Accuracy
*   **Fine-tuned DistilBERT:** 55.56% Accuracy
*   **Performance Delta:** -22.22% (Regression)

### Fine-Tuned Model Confusion Matrix
| True \ Predicted | analysis | hot_take | reactionary_vent |
| :--- | :---: | :---: | :---: |
| **analysis** | 0 | 0 | 4 |
| **hot_take** | 0 | 1 | 11 |
| **reactionary_vent** | 0 | 1 | 19 |

![Fine-Tuned Model Confusion Matrix](confusion_matrix.png)

### Error Analysis & Core Failure Modes
The fine-tuned model experienced a major **class collapse failure**. It overwhelmingly predicted `reactionary_vent` for 34 out of 36 test examples, completely failing to identify a single `analysis` post. 

1.  **Misclassifying Analysis (#1)**: A post detailing player tracking stats was predicted as `reactionary_vent`. This occurred because the comment was long and filled with exclamation points of surprise at the numbers, tricking DistilBERT's simple semantic head into prioritizing stylistic emotion over numerical content.
2.  **Misclassifying Hot Takes (#2)**: Aggressive trade demands were systematically swept into `reactionary_vent`. The model failed to differentiate between the structural syntax of a narrative opinion (`hot_take`) and a simple live-game reaction.
3.  **The Dominant Class Bias (#3)**: Because the raw scraped dataset natively contained a vastly higher volume of reactionary game-thread vents, the model minimized its training loss during its 3 epochs by simply defaulting to the majority class.

## 2. Reflections & Key Gaps
There is a stark divergence between intended learning and actual performance. I intended for the model to parse logical reasoning vs. emotional sentiment. Instead, the fine-tuned DistilBERT merely mapped surface-level text length and punctuation markers. 

To bridge this gap in the future, the dataset requires heavily stratified sampling to guarantee an even distribution of classes (e.g., exactly 70 examples of each class) rather than letting raw game-thread data drown out structured sports analysis.

## 3. AI Usage & Transparency Disclosure
*   **Instance 1 (Data Pipeline Modification)**: Overrode initial architectural advice to utilize a flat-rate subscription scraper, switching to a Pay-Per-Result model to conserve platform credits.
*   **Instance 2 (Error Diagnostics)**: Used an LLM to analyze the model's confusion matrix output and identify the math behind the class collapse failure mode.
