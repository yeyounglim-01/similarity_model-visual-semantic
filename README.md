# 💎 Trademark Similarity Analysis System
> Tip analyzes trademark similarity according to the three legal standards used in trademark infringement judgments: appearance, pronunciation, and concept.

AI-powered trademark similarity analysis system that detects potential trademark infringement by analyzing **semantic (conceptual) similarity** and *visual similarity* between trademarks.

This project combines **natural language understanding** and **computer vision models** to provide a more comprehensive trademark similarity evaluation.

---
# 📍 Overview

**Tip (Trademark Intelligence Platform)** is an AI-powered trademark monitoring system that analyzes similarity across **visual appearance, pronunciation, and conceptual meaning** — the three legal criteria used in trademark infringement judgment.

Unlike conventional services that rely on simple keyword searches or image matching, Tip applies AI-based analysis to provide more meaningful and legally relevant trademark similarity detection, helping brands identify potential infringement risks early.

---

# 🛡️ Key Features

* **Legal-Based Similarity Analysis** <br>
Analyzes trademark similarity based on the three legal criteria used in trademark law: visual appearance, pronunciation, and conceptual meaning.

* **Visual Similarity Analysis (Appearance)** <br>
Detects visually similar trademarks by analyzing logo structure, shapes, and design elements using deep learning models.

* **Semantic Similarity Analysis (Concept)** <br>
Evaluates conceptual similarity between trademarks using AI-generated descriptions and semantic embeddings.

* **Automated Similarity Scoring** <br>
Calculates similarity scores between target and candidate trademarks using vector-based comparison.

* **Modular AI Architecture** <br>
Designed with a modular structure to allow easy expansion of additional models such as phonetic similarity analysis.

---
# 🗂️ Project Structure
````
semanticmodel/
├── README.md

├── semantic_similarity/ # Semantic similarity model
│ ├── config.py
│ ├── trademark_analysis.py
│ ├── compare.py
│ ├── requirements.txt
│ ├── .env.example
│ ├── .gitignore
│ ├── README.md
│ ├── docs_SETUP_GUIDE_KO.md
│ ├── 00_GITHUB_UPLOAD_CHECKLIST.md
│ └── GITHUB_GUIDELINE.md

└── visual_similarity/ # Visual similarity model
├── .gitignore
├── README.md
├── model_utils.py
├── db_example.py
├── requirements.txt
└── resnet50_triplet_final.pth
````
Each module contains its own documentation explaining implementation details and usage.

---
# 💻 Models

## Semantic Similarity Model
The semantic similarity model analyzes the **conceptual meaning of trademarks** using text embeddings.

Process:

**1.** Generate text descriptions of trademark images <br>
**2.** Convert descriptions into semantic embeddings <br>
**3.** Compare embedding vectors using similarity metrics <br>
**4.** Calculate conceptual similarity scores <br>

More details are available in:
````
semantic_similarity/README.md
````
## Visual Similarity Model
The visual similarity model compares trademarks based on their **visual appearance.**
The model extracts visual features from trademark images and computes similarity scores between feature vectors.

Typical pipeline:

**1.** Detect trademark region <br>
**2.** Extract visual features using CNN <br>
**3.** Convert image into embedding vector <br>
**4.** Compute similarity between vectors <br>

More details are available in:
````
visual_similarity/README.md
````
# Technology Stack
* Python <br>
* PyTorch <br>
* OpenAI / Embedding Models <br>
* Computer Vision(CNN) <br>
* Vector Similarity Search
----
# Applications
This systen can be used for:
* Trademark infringement detection <br>
* Trademark monitoring systems <br>
* Intellectual property analysis <br>
* Brand protection services
----
# License
This project is licensed under the MIT License.

<img width="996" height="633" alt="image" src="https://github.com/user-attachments/assets/67407ac0-b9ab-45e0-9351-2f4b293344fd" />

