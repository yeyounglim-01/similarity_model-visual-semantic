# Visual Similarity Model

An image similarity measurement model using ResNet50-based Triplet Network.

## 📦 Components

| File Name                    | Description                                     |
| ---------------------------- | ----------------------------------------------- |
| `model_utils.py`             | Model loading, embedding generation, similarity calculation functions |
| `db_example.py`              | Database integration usage example              |
| `resnet50_triplet_final.pth` | Trained model weights (PyTorch)                 |

## 🚀 Installation

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Python Version

- **Recommended**: Python 3.8 or higher
- **Tested**: Python 3.9, 3.10, 3.11

### 3. GPU Support (Optional)

```bash
# CUDA-enabled PyTorch installation (Recommended)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

## 💡 Usage

### Basic Usage

```python
from model_utils import load_trained_model, get_embedding, get_cosine_similarity
import torch

# 1. Select device (GPU or CPU)
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

# 2. Load model
model = load_trained_model('resnet50_triplet_final.pth', device)

# 3. Convert image to 128-dimensional vector
embedding = get_embedding('image.jpg', model, device)
# embedding: numpy array, shape (128,)

# 4. Calculate similarity between two images
query_embedding = get_embedding('query.jpg', model, device)
db_embedding = get_embedding('db_image.jpg', model, device)
similarity = get_cosine_similarity(query_embedding, db_embedding)
# similarity: 0.0 ~ 1.0 (closer to 1 = more similar)
```

## 🔧 Model Details

### Architecture

- **Backbone**: ResNet50 (ImageNet pre-trained)
- **Embedding Dimension**: 128
- **Normalization**: L2 normalization (for cosine similarity)
- **Training Method**: Triplet Loss

### Input

- **Image Size**: 224×224 RGB
- **Preprocessing**: ImageNet standardization (mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])

### Output

- **Embedding**: 128-dimensional normalized vector
- **Similarity**: Range 0.0~1.0 (cosine similarity)

## 📊 Similarity Threshold Guide

| Score Range | Result | Description |
| --- | --- | --- |
| 0.7 ~ 1.0 | 🟢 Very High | Identical objects (automatic matching possible) |
| 0.3 ~ 0.7 | 🟡 Moderate | Similar with variations (manual review recommended) |
| 0.2 ~ 0.3 | 🟠 Boundary | Morphologically similar (candidate exposure) |
| Below 0.2 | 🔴 Low | Dissimilar |

## ⚙️ Important Notes

- The model is sensitive to color information
- Performance can be improved with batch processing for large-scale data
- GPU usage recommended for processing large datasets

## 📝 License

This project is freely available for use.
