# TakeMeter - r/scifi Discourse Classifier

AI201 Project 03 - Fine-tuning a text classifier to evaluate discourse quality in an online community.

## Community Choice

### r/scifi

This project uses the [r/scifi](https://www.reddit.com/r/scifi/) community as the dataset source.

I chose r/scifi because it contains a wide range of discussion styles about science fiction media (books, films, and TV shows). The discourse ranges from:

- Analytical breakdowns of worldbuilding, physics, and narrative structure  
- Interpretations of symbolism and thematic meaning  
- Strong subjective opinions and “hot takes”  
- Short emotional reactions to plot events or reveals  

This variation makes it a strong candidate for classification because “quality of discourse” is not uniform — posts differ in reasoning depth, intent, and structure.

These distinctions matter because members of the community often mix analysis and personal reaction in the same comment. Separating these modes helps clarify how arguments are formed and how subjective vs. structured discourse appears in fan discussions.

## Label Taxonomy

### Label 1: Analysis
Uses reasoning, evidence, comparisons, or domain knowledge (science fiction themes, worldbuilding, narrative structure).

**Examples:**
- *Dune’s political themes are more influential than its technology because later sci-fi borrowed its social structures more than its gadgets.*
- *The Expanse feels realistic because it consistently follows Newtonian physics.*

---

### Label 2: Interpretation
Explains meaning, symbolism, themes, or authorial intent without requiring external verification.

**Examples:**
- *The aliens represent humanity’s fear of the unknown.*
- *The ending suggests that progress comes at the cost of identity.*

---

### Label 3: Opinion / Hot Take
Strong subjective judgment with little structured justification.

**Examples:**
- *Dune is so overrated.*
- *Foundation is the most boring sci-fi series ever written.*

---

### Label 4: Reaction
Immediate emotional response to a specific moment or event.

**Examples:**
- *That ending blew my mind.*
- *I was so disappointed by season 2.*

## Data Collection & Annotation

### Source

All data was collected from **r/scifi discussion threads**, primarily focusing on comment sections discussing sci-fi books, films, and TV shows.

Only comments were included (not standalone posts) to ensure:
- More consistent discourse structure
- Higher density of “takes”
- Better comparability between examples

---

### Collection Process

- Manual browsing of Reddit threads
- Copying individual comments into a spreadsheet
- Assigning labels based on definitions in `planning.md`
- Filtering out low-information comments (e.g., “I agree”, “great point”)

---

### Dataset Constraints

- Minimum length: 10 words  
- Maximum length: 150 words  
- Must contain at least one of:
  - explanation
  - interpretation
  - opinion
  - emotional reaction  

---

### Dataset Size & Distribution

> ⚠️ Replace once final dataset is complete

| Label              | Count |
|-------------------|-------|
| Analysis          | TBD   |
| Interpretation    | TBD   |
| Opinion/Hot Take  | TBD   |
| Reaction          | TBD   |

Total: 200+ examples

---

### Difficult Labeling Cases

#### Example 1
> “The Expanse feels more realistic than Star Trek because it follows physics, but I just prefer it overall.”

**Decision:** Opinion / Hot Take  
Reason: Although it contains analytical reasoning, the primary intent is preference expression.

---

#### Example 2
> “The ending of Dune represents humanity’s fear of losing control to larger systems like religion and politics.”

**Decision:** Interpretation  
Reason: Focuses on thematic meaning rather than structured argument.

---

#### Example 3
> “That reveal in Foundation was insane... I had to pause the episode.”

**Decision:** Reaction  
Reason: Pure emotional response without explanatory structure.

## Fine-Tuning Approach

### Base Model

- `distilbert-base-uncased`
- HuggingFace Transformers
- Fine-tuned using Google Colab (T4 GPU)

---

### Training Setup

- Train/validation/test split: 70/15/15
- Training framework: HuggingFace `Trainer`
- Loss: cross-entropy classification loss
- Max sequence length: 256 tokens

---

### Key Hyperparameter Decision

**Learning rate: 2e-5**

This was chosen because:
- It is the standard fine-tuning rate for BERT-style models
- Lower rates would slow learning on a small dataset (~200 examples)
- Higher rates risk instability and overfitting

## Baseline (Zero-Shot LLM)

### Model

- Groq API: `llama-3.3-70b-versatile`

---

### Prompt Strategy

The model is prompted with:

- Task description (classify r/scifi comments)
- Full label definitions
- One example per label
- Instruction: output ONLY the label name

The prompt is designed to enforce strict output formatting because evaluation requires exact label matching.

---

### Collection Method

- Each test example is passed individually to the Groq API
- Temperature set to 0 for deterministic output
- Responses are parsed and mapped to label IDs
- Unparseable outputs are excluded from evaluation

## Evaluation Report

> ⚠️ To be filled after training

### Fine-tuned Model Metrics

**Accuracy:** TBD  

**Per-class metrics:**
- Precision: TBD
- Recall: TBD
- F1: TBD

---

### Baseline Model Metrics

**Accuracy:** TBD  

**Per-class metrics:**
- Precision: TBD
- Recall: TBD
- F1: TBD

## Confusion Matrix (Fine-Tuned Model)

> Replace with actual results after training

| True \ Pred | Analysis | Interpretation | Opinion | Reaction |
|-------------|----------|----------------|--------|----------|
| Analysis    | TBD      | TBD            | TBD    | TBD      |
| Interpretation | TBD  | TBD            | TBD    | TBD      |
| Opinion     | TBD      | TBD            | TBD    | TBD      |
| Reaction    | TBD      | TBD            | TBD    | TBD      |

## Error Analysis (Wrong Predictions)

> Replace with actual test errors after training

### Example 1
- **Text:** TBD  
- **True:** TBD  
- **Predicted:** TBD  
- **Analysis:** TBD  

### Example 2
- **Text:** TBD  
- **True:** TBD  
- **Predicted:** TBD  
- **Analysis:** TBD  

### Example 3
- **Text:** TBD  
- **True:** TBD  
- **Predicted:** TBD  
- **Analysis:** TBD  

## Sample Model Predictions

> 3–5 examples from test set

| Text | True Label | Predicted | Confidence |
|------|------------|----------|------------|
| TBD  | TBD        | TBD      | TBD        |
| TBD  | TBD        | TBD      | TBD        |
| TBD  | TBD        | TBD      | TBD        |

### One Correct Prediction (Explanation Required)

> Choose one correct prediction and explain why it worked

## Reflection: Model vs Intent

This section will analyze the gap between:

- Intended label definitions
- What the model actually learned

To be filled after evaluation.

Key areas of interest:
- whether Analysis vs Interpretation is separable
- whether Reaction is over-predicted
- whether Opinion vs Interpretation boundary collapses

## Spec Reflection

### What helped from the spec

- Clear requirement for mutually exclusive labels forced precise taxonomy design
- Explicit edge-case handling improved annotation consistency

### Where implementation diverged

- TBD during dataset construction / model evaluation

## AI Usage

### Instance 1: Label Design Stress Testing
Used AI to generate borderline r/scifi comments between:
- Analysis vs Interpretation
- Opinion vs Reaction

**Outcome:**
Helped refine edge-case rules, especially when reasoning and opinion co-occur.

---

### Instance 2: Failure Pattern Analysis (planned)
AI will be used after evaluation to:
- Analyze misclassified test examples
- Identify confusion clusters between labels
- Suggest systematic failure patterns

All patterns will be manually verified before inclusion in final report.

---

### Annotation Assistance (if used)

> TBD — disclose whether AI was used to pre-label dataset and how much was corrected manually.