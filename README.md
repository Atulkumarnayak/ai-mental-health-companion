# AI-Powered Personalized Mental Health Companion

An interactive digital platform that analyzes textual input to recognize user emotions and generate empathetic, context-sensitive responses using fine-tuned DistilBERT transformer model.

## Features

- **Emotion Classification**: Recognizes 6 basic emotions (joy, sadness, anger, fear, disgust, surprise) with 84% accuracy
- **Personalized Recommendations**: CBT-based coping strategies tailored to each emotion
- **Real-time Inference**: Sub-200ms latency for seamless conversation
- **Privacy-First Design**: No personal data storage in the demo; can be wired to encrypted MongoDB in production
- **Visual Analytics**: Emotion trends and distributions over time
- **Ethical AI Framework**: Transparent, explainable, and bias-mitigated
- **Chat Therapist API**: `/api/chat` combines emotion analysis, CBT hints, crisis detection, and medical assistance cues
- **Wellness Modules**: Mood tracking dashboard, guided breathing, and private journaling in the Streamlit frontend

## Architecture

```
┌─────────────────┐
│  Streamlit UI   │
└────────┬────────┘
         │
┌────────▼────────┐
│   FastAPI API   │
└────────┬────────┘
         │
┌────────▼────────┐
│  DistilBERT     │
│  Emotion Model  │
└────────┬────────┘
         │
┌────────▼────────┐
│ Recommendation  │
│    Engine       │
└─────────────────┘
```

## Project Structure

```
.
├── data/                   # Data directory
│   ├── raw/               # Raw datasets
│   └── processed/         # Processed datasets
├── models/                 # Trained model files
├── src/
│   ├── data/              # Data processing scripts
│   ├── training/          # Model training scripts
│   ├── inference/         # Model inference utilities
│   ├── recommendations/   # Recommendation engine
│   ├── api/               # FastAPI backend
│   └── frontend/          # Streamlit frontend
├── tests/                 # Unit tests
├── docker/                # Docker configurations
├── requirements.txt       # Python dependencies
└── README.md             # This file
```

## Installation

### Prerequisites

- Python 3.10+
- CUDA-capable GPU (optional, for training)
- Docker (optional, for containerized deployment)

### Setup

1. **Clone the repository**
```bash
git clone <repository-url>
cd "AI-Powered Personalized Mental Health Companion"
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords')"
```

4. **Download dataset**
   - The system uses the DAIR-AI Emotion Dataset
   - Place dataset in `data/raw/` directory
   - Or use the provided data download script

## Usage

### Training the Model

1. **Preprocess data**
```bash
python src/data/preprocess.py --input data/raw/emotion_dataset.csv --output data/processed/
```

2. **Train DistilBERT model**
```bash
python src/training/train.py --data_dir data/processed/ --output_dir models/ --epochs 5 --batch_size 32 --learning_rate 2e-5
```

### Running the Backend API

```bash
uvicorn src.api.main:app --host 0.0.0.0 --port 8000 --reload
```

API will be available at `http://localhost:8000`
- Documentation: `http://localhost:8000/docs`
- Health check: `http://localhost:8000/healthcheck`

### Running the Frontend

```bash
streamlit run src/frontend/app.py
```

Frontend will be available at `http://localhost:8501`

### Additional Wellness & Medical Assistance APIs

New endpoints are exposed in `src/api/main.py`:

- `POST /api/chat` – Conversational therapist with emotion, CBT, crisis signal detection, and optional medical support panel payload.
- `POST /api/mood-log` & `GET /api/mood-log` – Log and fetch mood check-ins.
- `POST /api/journal` – Analyze a journal entry and return emotional summaries and patterns.
- `GET /api/emotion-analysis` – Aggregate insights across mood logs and journals for dashboards.
- `POST /api/symptom-support` – Non‑diagnostic guidance for symptom descriptions plus nearby support and emergency contacts.
- `POST /api/nearby-medical-support` – Placeholder for map-backed hospital/clinic lookup.

For a MongoDB schema blueprint, see `docs_mongodb_schema.md`. Before production use, add JWT-based auth, encrypt journal content at rest, and wire map lookups to Google Maps or OpenStreetMap.

### Docker Deployment

1. **Build Docker image**
```bash
docker build -t mental-health-companion .
```

2. **Run container**
```bash
docker run -p 8000:8000 -p 8501:8501 mental-health-companion
```

## API Endpoints

### POST /predict
Predicts emotion from user text input.

**Request:**
```json
{
  "text": "I feel really anxious about tomorrow's presentation"
}
```

**Response:**
```json
{
  "emotion": "fear",
  "confidence": 0.89,
  "probabilities": {
    "joy": 0.02,
    "sadness": 0.08,
    "anger": 0.01,
    "fear": 0.89,
    "disgust": 0.00,
    "surprise": 0.00
  },
  "recommendation": "Use grounding 5-4-3-2-1 method; remind yourself of prior resilience.",
  "explanation": "The model detected words indicating anxiety and worry."
}
```

### GET /healthcheck
Returns API health status.

**Response:**
```json
{
  "status": "healthy",
  "model_loaded": true,
  "timestamp": "2025-01-XX XX:XX:XX"
}
```

## Model Performance

| Metric | Value |
|--------|-------|
| Accuracy | 84% |
| Precision | 83% |
| Recall | 84% |
| F1-Score | 0.83 |
| Inference Latency | <200ms |

## Ethical Considerations

- **Privacy**: No personal identifiers stored; session data auto-expires
- **Transparency**: Model predictions include confidence scores and explanations
- **Bias Mitigation**: Class balancing and fairness audits implemented
- **User Autonomy**: Clear disclaimers and opt-out mechanisms
- **Security**: TLS encryption, container isolation, regular security audits

## Contributing

This is a research project. For contributions:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with detailed description

## License

This project is open-source and available for research and educational purposes.

## Citation

If you use this work, please cite:
```
A. Tyagi et al., "AI-Powered Personalized Mental Health Companion," 
Ajay Kumar Garg Engineering College Technical Report, 2025.
```

## Contact

For questions or issues, please open an issue on the repository.

