# Assistive Visual Question Answering with Answerability Detection

This repository contains the complete Machine Learning final project pipeline for **Assistive Visual Question Answering with Answerability Detection**.

The project uses the **VizWiz VQA dataset** to build a practical Visual Question Answering system for assistive use. The system predicts an answer from an image and question, then decides whether the answer is reliable enough to trust.

## Project Overview

Visual Question Answering systems usually return an answer for every image-question pair. In assistive settings, this can be risky because visually impaired users may upload images that are blurry, dark, incomplete or difficult to interpret.

This project adds an answerability detection step so the model can reject uncertain cases instead of always guessing.

Final system output:

```text
Image + Question -> Predicted Answer + Confidence Score + Answerability Decision
```

## Team Members

- Syed Muhammad Huzaifa Ali
- Akash Kumar Shaw
- Ishtpreet Singh

## Dataset

Dataset used: **VizWiz VQA**

Official dataset page: https://vizwiz.org/tasks-and-datasets/vqa/

Used splits:

- Training annotations and images
- Validation annotations and images

The test split was not used for reported metrics because test answers are hidden.

## Project Pipeline

The notebook is divided into three main phases.

### Phase 1: Dataset Preparation and Feature Extraction

Owner: Syed Muhammad Huzaifa Ali

Main tasks:

- Downloaded and organized the VizWiz dataset
- Loaded train and validation annotations
- Built metadata dataframes
- Normalized answers and selected majority answers
- Checked for missing images
- Performed exploratory data analysis
- Created a 50 percent stratified subset
- Used frozen CLIP ViT-B/32 for feature extraction
- Saved image features, question features and metadata files

Output feature shapes:

```text
Train image features:      10261 x 512
Train question features:   10261 x 512
Validation image features: 2159 x 512
Validation question features: 2159 x 512
```

### Phase 2: Multimodal Answer Prediction

Owner: Akash Kumar Shaw

Main tasks:

- Built answer vocabulary using top 50 answers plus an `other` class
- Trained question-only baseline
- Trained image-only baseline
- Trained image-question fusion model
- Generated predicted answers and confidence scores

Validation accuracy:

| Model | Validation Accuracy |
|---|---:|
| Question-only baseline | 53.50% |
| Image-only baseline | 62.95% |
| Image + question fusion model | 68.60% |

### Phase 3: Answerability Detection

Owner: Ishtpreet Singh

Main tasks:

- Used predicted answers and confidence scores
- Built confidence-only baseline detector
- Built improved rule-based answerability detector
- Compared accuracy, precision, recall and F1 score
- Generated confusion matrices and final result files

Answerability detection results:

| Method | Accuracy | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| Confidence-only baseline | 0.6378 | 0.6719 | 0.9279 | 0.7797 |
| Improved rule-based detector | 0.6554 | 0.9175 | 0.5413 | 0.6810 |

The improved detector prioritizes precision because in an assistive system, refusing to answer is safer than giving a wrong answer.

## Repository Structure

Suggested structure:

```text
.
├── Complete_Pipeline_Machine_Learning_Final_Project.ipynb
├── README.md
├── report/
│   └── final_report.pdf
├── figures/
│   ├── baseline_vs_main_model_comparison.png
│   ├── baseline_detector.png
│   ├── improved_detector_confusion_matrix.png
│   └── final_comparison_chart.png
└── .gitignore
```

Large dataset files, extracted embeddings and model files should not be uploaded to GitHub.

## How to Run

1. Open the notebook in Google Colab.
2. Change runtime to GPU.
3. Run the notebook cells from top to bottom.
4. The dataset will be downloaded directly inside Colab.
5. The pipeline will create metadata, features, predictions and results.

Recommended runtime:

```text
Google Colab with GPU enabled
```

Required Python libraries:

```text
transformers
huggingface_hub
torch
torchvision
pillow
tqdm
numpy
pandas
matplotlib
scikit-learn
```

## GitHub Notebook Rendering Note

If GitHub shows an error instead of displaying the notebook, the issue is usually caused by saved Colab widget outputs or progress bar outputs inside the `.ipynb` file.

To reduce this issue, add the following code before loading CLIP in the section named **1.10 Install Dependencies and Load CLIP**:

```python
import os
os.environ["HF_HUB_DISABLE_PROGRESS_BARS"] = "1"

from huggingface_hub.utils import disable_progress_bars
disable_progress_bars()
```

Then rerun the CLIP loading cell, save the notebook and upload it again.

Alternative option:

```text
Runtime -> Restart session
Edit -> Clear all outputs
Run only the required cells
Save a clean copy for GitHub
```

If you want to keep outputs visible, avoid saving interactive widget outputs. Static text outputs, tables and images usually render correctly on GitHub.

## Important Files Generated by the Notebook

Phase 1 outputs:

```text
image_features_train.npy
question_features_train.npy
image_features_val.npy
question_features_val.npy
metadata_train_subset.csv
metadata_val_subset.csv
```

Phase 2 outputs:

```text
predictions.csv
confidence_scores.csv
```

Phase 3 outputs:

```text
answerability_results.csv
failure_cases.csv
```

These generated files are not required in the GitHub repository unless specifically requested. They can be regenerated by running the notebook.

## Key Findings

- The VizWiz dataset contains a meaningful number of unanswerable examples.
- CLIP features provide a practical way to represent both images and questions.
- The fusion model performs better than single-modality baselines.
- Confidence alone is not enough for answerability detection.
- A stricter rule-based detector improves reliability by reducing unsafe answer acceptance.

## Limitations

- The answer classifier is limited to the top 50 answers plus `other`.
- CLIP was used as a frozen feature extractor rather than being fine-tuned.
- OCR-heavy images, blurry labels, rare objects and ambiguous images remain challenging.
- The system was designed for a Colab-friendly workflow, so larger training may improve results.

## Future Work

- Add OCR for product labels and text-heavy images
- Fine-tune a stronger vision-language model
- Use the full dataset with more compute
- Train a learned answerability classifier
- Build a mobile or voice-based assistive interface

## References

- VizWiz VQA dataset: https://vizwiz.org/tasks-and-datasets/vqa/
- CLIP model: https://huggingface.co/openai/clip-vit-base-patch32
