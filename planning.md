# TakeMeter Project Planning

## Community Choice

> [!IMPORTANT]
> My chosen community is [r/scifi](https://www.reddit.com/r/scifi/)

### Why This Community?

**Why I chose it:** I chose r/scifi because it has a mix of analytical discussions, theme interpretations, and informal opinions about science fiction books, films, and TV shows. Unlike communities focused only on news or facts, sci-fi discussions often include subjective judgments about meaning, quality, and plausibility, which makes it useful for studying discourse quality.

**Why discourse varies enough for classification:** The discussions in this community also vary a lot in structure and depth. Some comments are detailed analyses of worldbuilding, scientific plausibility, or narrative structure. Others focus on interpreting symbolism or what the author might have intended. Many are also simple personal opinions or emotional reactions to a story or ending. This variety creates clear but sometimes overlapping categories of “takes,” which makes it a good fit for classification work because it requires careful label definitions and includes many borderline cases.

**Why the distinctions matter to members:** These differences matter to members because they reflect different ways of engaging with science fiction. Some people focus on serious critique and analysis, while others are more interested in interpretation or just sharing reactions and preferences. Separating these types of discourse helps make it clearer what kind of contribution someone is making in a discussion and highlights the difference between reasoned critique and subjective response, which is a common tension in fan communities.

## Label Taxonomy

### Label 1: Analysis

**Definition:** Uses reasoning, evidence, comparisons, literary themes, worldbuilding details, or genre knowledge to support a claim.

> **Example 1:** Dune's political themes are more influential than its technology because later sci-fi borrowed its social structures more than its gadgets.

> **Example 2:** The Expanse feels realistic because it consistently follows Newtonian physics.

---

### Label 2: Interpretation

**Definition:** Offers a reading or explanation of a work's meaning, symbolism, themes, or authorial intent.

> **Example 1:** The aliens represent humanity's fear of the unknown.

> **Example 2:** The ending suggests that progress comes at the cost of identity.

---

### Label 3: Opinion / Hot Take

**Definition:** Strong preference or judgment with little substantive justification.

> **Example 1:** Dune is so overrated.

> **Example 2:** Foundation is the most boring sci-fi series ever written.

---

### Label 4: Reaction

**Definition:** Immediate emotional response, excitement, disappointment, surprise, nostalgia, etc.

> **Example 1:** That ending blew my mind.

> **Example 2:** I was so disappointed by season 2.

## Hard Edge Cases

### Edge Case 1

**Post Type:** “The Expanse feels more realistic than most sci-fi because it follows Newtonian physics, but honestly I just like it more than Star Trek because it feels less cheesy.”

**Possible Labels:**
* Analysis
* Opinion / Hot Take
* Interpretation

**Decision Rule:** If a comment uses specific reasoning or domain knowledge to support part of its claim, it starts as a Analysis. However, if the reasoning shifts into personal preference or value judgment (“I just like it more,” “it feels better,” “more enjoyable”), the intent becomes evaluative rather than explanatory.

In that case, the physics argument is analytical, but it is used as support for a subjective preference. Because the primary purpose of the post is expressing preference rather than building a structured argument, label it **Opinion / Hot Take**.

---

### Edge Case 2

**Post Type:** “The ending of Dune represents humanity’s fear of surrendering control to systems bigger than itself, especially religion and politics.”

**Possible Labels:**
* Interpretation
* Analysis
* Opinion / Hot Take

**Decision Rule:** If the comment explains meaning, symbolism, themes, or author intent without requiring external verification, it is **Interpretation**. If it relies on structured evidence or comparative reasoning across works, it becomes **Analysis**.

In that case, the claim is about symbolic meaning and thematic reading rather than proving something empirically. There is no argumentative structure or supporting evidence beyond textual reading, so it should be labeled **Interpretation**.

---

### Edge Case 3

**Post Type:** “That reveal in Foundation was insane... I literally had to pause the episode.”

**Possible Labels:**
* Reaction
* Opinion / Hot Take
* Interpretation

**Decision Rule:** If the primary function of the comment is emotional response to a specific moment (surprise, excitement, frustration, etc.), label it **Reaction**, even if it contains evaluative language like “good,” “bad,” or “insane.”

If the comment attempts to justify why the moment is meaningful or connects it to broader themes, it shifts toward Opinion or Interpretation. In this case, there is no reasoning or thematic explanation, only an immediate emotional response so label it **Reaction**.

## Data Collection Plan

### Source

I will collect data from [r/scifi](https://www.reddit.com/r/scifi/), focusing specifically on top-level comments and replies within discussion threads about science fiction books, films, and TV shows. I will try to avoid standalone posts and focus on comment threads because they tend to contain more direct “takes” and are more consistent in length and structure.

### Collection Method

I will collect the data manually by browsing discussion threads and copying individual comments into a spreadsheet. Each comment will be read in context and then assigned a label based on the definitions in my planning document.

To ensure consistency and reduce noise in the dataset, I will only include comments that meet the following constraints:

* Minimum length: 10 words
* Maximum length: 150 words
* Must contain a clear opinion, explanation, interpretation, or reaction (no low-information comments such as “I agree” or “great point”)

Each entry will include:

* Comment text
* Assigned label

### Target Distribution

> I will aim for a balanced dataset to prevent class imbalance issues during training.

| Label              | Target Count  |
|:------------------:|:-------------:|
| Analysis           | 25%           |
| Interpretation     | 25%           |
| Opinion / Hot Take | 25%           |
| Reaction           | 25%           |

Total: 200 comments

This even distribution ensures the model learns all four discourse types equally rather than overfitting to the most common category.

### Handling Imbalance

If one label becomes overrepresented during collection, I will intentionally shift browsing toward threads that are more likely to contain underrepresented discourse types.

For example:

* Analytical comments are more common in technical discussion threads
* Interpretation appears more in thematic or “what does this mean” discussions
* Opinion / Hot Take is more common in recommendation or ranking threads
* Reaction is more common in episode release or spoiler discussion threads

If I cannot naturally balance the dataset through collection, I will continue sampling until all labels reach at least ~20% of the dataset. No label will not exceed ~35% of the total dataset to avoid majority-class bias.

## Evaluation Plan

### Metrics

* Accuracy
* Precision
* Recall
* F1 Score
* Confusion Matrix

### Why These Metrics?

Accuracy provides an overall measure of how often the model correctly classifies comments across all four discourse categories. However, because this is a multi-class classification task with potentially uneven difficulty across labels, accuracy alone is not sufficient.

Precision and recall are included to evaluate how well the model performs on each individual label. Precision helps measure whether the model is over-predicting a specific class (e.g., labeling many comments as “Analysis” incorrectly), while recall measures whether the model is successfully capturing all relevant examples of a given class.

F1 score is used as the primary per-class metric because it balances precision and recall, making it useful for comparing performance across labels that may differ in frequency or ambiguity.

The confusion matrix is included to identify systematic misclassifications between labels (e.g., whether the model confuses Interpretation and Analysis), which is critical for understanding whether the model has learned meaningful distinctions or is relying on superficial cues.

### Definition of Success

The model will be considered successful if:

* Overall accuracy ≥ **75%**
* Per-class F1 score ≥ **0.65** for all labels
* The fine-tuned DistilBERT model outperforms the zero-shot Groq baseline on overall accuracy and at least 3 of 4 labels
* No single class dominates predictions (i.e., the model does not collapse into predicting one label most of the time)

Additionally, success includes qualitative behavior: the confusion matrix should show that errors are distributed rather than concentrated entirely between two indistinguishable labels, indicating that the taxonomy is learnable.

## AI Tool Plan

### Label Stress Testing

I will use an AI model to generate synthetic or borderline comments based on my label definitions (Analysis, Interpretation, Opinion / Hot Take, Reaction). These generated examples will be specifically designed to sit near decision boundaries (e.g., Analysis vs Interpretation or Opinion vs Reaction).

I will use these examples to test whether my labeling rules are sufficiently precise and whether edge cases are consistently classifiable. If I cannot confidently assign a label to these generated examples, I will refine my label definitions before continuing annotation.

### Annotation Assistance

Yes, I will use AI for **pre-labeling assistance** on a subset of the dataset.

The AI will assign preliminary labels to comments, but every label will be manually reviewed and corrected before being finalized. AI output will not be accepted without verification. This is strictly used to speed up initial labeling and improve consistency, not to replace human annotation.

### Failure Analysis

After model evaluation, I will use an AI tool to analyze misclassified examples from the test set. I will provide the model with incorrectly labeled comments along with their true and predicted labels.

The AI will help identify patterns in errors, such as:

* Confusion between specific label pairs (e.g., Analysis vs Interpretation)
* Sensitivity to comment length
* Misclassification of sarcasm or informal language
* Over-reliance on keywords instead of meaning

These insights will then be manually verified by reviewing the original comments to ensure the patterns are real and not hallucinated. This analysis will be used in the final evaluation report to explain systematic model weaknesses.

## Stretch Features (Optional)

### Inter-Annotator Reliability

Status:
Plan:

### Confidence Calibration

Status:
Plan:

### Error Pattern Analysis

Status:
Plan:

### Deployed Interface

Status:
Plan: