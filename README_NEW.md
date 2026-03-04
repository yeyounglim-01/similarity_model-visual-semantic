# Trademark Semantic Similarity Analysis

AI-powered trademark analysis system using Azure OpenAI and semantic embeddings to compare trademark images and detect similarity.

## 🎯 Overview

This project analyzes trademark images using GPT-5.1 vision capabilities and generates semantic embeddings to:
- Generate detailed text descriptions from trademark images
- Create semantic embeddings for similarity comparison
- Calculate similarity scores between target and candidate trademarks
- Generate comprehensive analysis reports

### Key Features
- **Vision-based Analysis**: Uses Azure OpenAI's GPT-5.1 to analyze trademark images
- **Semantic Embeddings**: Converts descriptions to embeddings using text-embedding-3-large
- **Similarity Scoring**: Calculates cosine similarity between trademark embeddings
- **Batch Processing**: Handles multiple trademark comparisons efficiently
- **Configurable**: Easy environment-based configuration

---

## 📋 Requirements

- Python 3.8+
- Azure OpenAI API access
- Internet connection for API calls

## 🚀 Installation

### 1. Clone the repository
```bash
git clone https://github.com/your-username/semanticmodel.git
cd semanticmodel
```

### 2. Create Python virtual environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure environment variables
Copy the example environment file and update with your Azure OpenAI credentials:

```bash
cp .env.example .env
```

Then edit `.env` file with your actual values:
```
# Azure OpenAI Configuration
AZURE_API_KEY=your_actual_api_key_here
AZURE_API_VERSION=2024-05-01-preview
AZURE_ENDPOINT=https://your-resource-name.openai.azure.com/

# Model Configuration
MODEL_NAME=gpt-5.1-chat
EMBEDDING_MODEL=text-embedding-3-large

# Path Configuration
TARGET_DIR=./test_images/target
CANDIDATE_DIR=./test_images/candidates
OUTPUT_FILE=data.jsonl

# Rate Limit Configuration (in seconds)
SLEEP_TIME=0.5
```

---

## 📁 Project Structure

```
semanticmodel/
├── src/
│   ├── config.py              # Configuration and environment setup
│   ├── trademark_analysis.py  # Main analysis logic with image processing
│   └── compare.py             # Similarity comparison and analysis
│
├── test_images/               # Test trademark images folder
│   ├── target/                # Reference trademark images
│   └── candidates/            # Candidate trademark images for comparison
│
├── .env.example               # Environment variables template
├── .gitignore                 # Git ignore rules
├── README.md                  # This file
├── requirements.txt           # Python dependencies
└── GITHUB_GUIDELINE.md        # GitHub upload guide
```

---

## 💡 Usage

### 1. Analyze Trademark Images

Run the main trademark analysis script:

```bash
python src/trademark_analysis.py
```

**Input**: Uses images from directories defined in `.env`:
- `TARGET_DIR`: Reference trademark image(s)
- `CANDIDATE_DIR`: Candidate trademark images to compare against

**Output**: `data.jsonl` file containing:
- Image file names
- Generated descriptions
- Text embeddings

### 2. Compare Trademarks

Compare similarity between analyzed trademarks:

```python
from src.compare import analyze_similarity

# Analyze similarity from JSONL data
results = analyze_similarity("data.jsonl")

# results format:
# {
#   "TARGET_logo.jpg": [
#       ("candidate1.jpg", 0.95),
#       ("candidate2.jpg", 0.87),
#       ...
#   ]
# }
```

### 3. Generate Analysis Report

The system automatically generates CSV reports:

```
보호상표,유사상표,유사도
TARGET_logo.png,candidate1.png,0.95
TARGET_logo.png,candidate2.png,0.87
```

---

## 🔧 Module Documentation

### trademark_analysis.py

**Main Functions:**

- `encode_image(image_path: str) -> str`
  - Encodes image to base64 format for API submission
  - Input: Image file path
  - Output: Base64 encoded string

- `generate_description(image_path: str) -> str`
  - Generates detailed Korean description of trademark image
  - Uses GPT-5.1 vision model
  - Returns 300-character limit description

- `get_embedding(text: str) -> List[float]`
  - Converts text description to embedding vector
  - Uses text-embedding-3-large model
  - Returns ~3072-dimensional vector

- `run_test()`
  - Main execution function
  - Processes all images in configured directories
  - Outputs results to JSONL file

### compare.py

**Main Functions:**

- `calculate_similarity(target_embedding: List[float], candidate_embeddings: Dict[str, List[float]]) -> List[Tuple[str, float]]`
  - Calculates cosine similarity scores
  - Sorts results by similarity (descending)
  - Returns: List of (filename, score) tuples

- `load_from_jsonl(file_path: str) -> Tuple[List[Dict], List[Dict]]`
  - Loads analysis data from JSONL file
  - Separates target and candidate data
  - Removes duplicate entries

- `analyze_similarity(jsonl_file: str) -> Dict`
  - Complete similarity analysis workflow
  - Returns: Dictionary mapping targets to ranked similar candidates

### config.py

- Loads environment variables from `.env` file
- Validates required configuration (API_KEY, ENDPOINT)
- Exports configuration constants for use in other modules

---

## 📊 Output Format

### data.jsonl
Each line is a JSON object:
```json
{
  "file_name": "TARGET_logo.jpg",
  "description": "Trademark image with specific color scheme and design elements...",
  "embedding": [0.123, 0.456, -0.789, ...],
  "is_target": true
}
```

### CSV Results
Format: `[보호상표, 유사상표, 유사도]`
- 보호상표: Target trademark filename
- 유사상표: Candidate trademark filename  
- 유사도: Similarity score (0.00 - 1.00)

---

## ⚙️ Configuration Details

### Rate Limiting
- `SLEEP_TIME`: Delay between API calls (default: 0.5 seconds)
- Important for avoiding API throttling with large image batches

### Model Selection
- `MODEL_NAME`: Vision model for image analysis (gpt-5.1-chat)
- `EMBEDDING_MODEL`: Text embedding model (text-embedding-3-large)

### Paths
- `TARGET_DIR`: Directory with reference trademark(s)
- `CANDIDATE_DIR`: Directory with candidate trademarks
- `OUTPUT_FILE`: Output file for embeddings data

---

## 🔐 Security

### Important Security Practices

1. **Never commit .env file** - Contains sensitive API credentials
2. **.env.example file** - Use this as template, no real credentials
3. **Environment Variables** - All sensitive data via `.env` file
4. **.gitignore** - Configured to prevent accidental commits

### API Key Management
- Store Azure OpenAI API key in `.env` file only
- Never hardcode credentials in source code
- Rotate keys regularly for production use

---

## 📈 Performance Notes

- **Large image batches**: May take significant time due to API calls
- **API costs**: Each image analysis and embedding incurs API charges
- **Rate limits**: Adjust `SLEEP_TIME` based on Azure API quota
- **Memory**: Embeddings are stored in JSONL (only ~3KB per entry)

---

## 🐛 Troubleshooting

### Common Issues

**Issue**: "AZURE_API_KEY was not configured properly"
- **Solution**: Check `.env` file exists and contains valid AZURE_API_KEY

**Issue**: "AZURE_ENDPOINT was not configured properly"  
- **Solution**: Verify AZURE_ENDPOINT URL is correct in `.env` file

**Issue**: API connection timeout
- **Solution**: Check internet connection and increase SLEEP_TIME in `.env`

**Issue**: Image not found errors
- **Solution**: Verify TARGET_DIR and CANDIDATE_DIR paths contain valid image files

---

## 📝 License

[Add Your License Here - e.g., MIT, Apache 2.0, etc.]

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

---

## 📧 Support

For questions or issues, please open an issue on GitHub: [your-repo-url]

---

## Version History

- **v1.0** (2026-03-04): Initial release
  - Trademark image analysis
  - Similarity comparison
  - CSV report generation
