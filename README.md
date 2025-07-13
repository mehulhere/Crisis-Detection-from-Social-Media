# Real-time Crisis Detection Model Using Tweets

## Overview

An AI-powered tool that analyzes real-time tweets to detect crisis events like riots, civil unrest, and natural disasters, providing live information to people in affected areas. This system combines machine learning with real-time data collection to enhance situational awareness during emergency situations.

## Key Features

- **Real-time Tweet Analysis**: Continuous monitoring and classification of tweets for crisis detection
- **High Accuracy**: Machine learning model achieving 93% accuracy in crisis/non-crisis classification
- **GPS-based Filtering**: Location-aware crisis detection for targeted alerts
- **Interactive Map Interface**: Visual representation of crisis events with geographic context
- **Automated Data Collection**: Selenium-powered tweet scraping for real-time data ingestion
- **Multi-model Ensemble**: Combines multiple ML algorithms for improved reliability

## Technical Architecture

### Machine Learning Models
- **Random Forest**: Primary ensemble method (92.24% accuracy)
- **Support Vector Machine**: High-performance classifier (91.43% accuracy)
- **XGBoost**: Gradient boosting for complex pattern recognition (91.11% accuracy)
- **Logistic Regression**: Baseline linear classifier (91.07% accuracy)

### Data Processing Pipeline
1. **Data Collection**: Automated tweet scraping using Selenium
2. **Text Preprocessing**: Cleaning, normalization, and feature extraction
3. **Feature Engineering**: TF-IDF vectorization and dimensionality reduction
4. **Model Training**: Ensemble learning with hyperparameter optimization
5. **Real-time Classification**: Live tweet analysis and crisis detection

## Dataset

The model is trained on a comprehensive dataset of 247,000 tweets:
- **Crisis tweets**: 129,000 samples
- **Non-crisis tweets**: 118,000 samples
- **Sources**: Multiple publicly available datasets including disaster-related tweets from various platforms

## Installation

### Prerequisites
- Python 3.8+
- Chrome browser (for Selenium)
- Required Python packages (see requirements.txt)

### Setup
```bash
# Clone the repository
git clone <repository-url>
cd crisis-detection-model

# Install dependencies
pip install -r requirements.txt

# Download NLTK data
python -c "import nltk; nltk.download('stopwords'); nltk.download('wordnet'); nltk.download('punkt')"

# Set up Chrome driver for Selenium
# Download ChromeDriver and place in PATH or project directory
```

## Usage

### Training the Model
```python
# Load and preprocess data
from src.data_preprocessing import load_and_clean_data
from src.model_training import train_models

# Load dataset
data = load_and_clean_data('data/Combined_dataset.csv')

# Train multiple models
models = train_models(data)
```

### Real-time Crisis Detection
```python
from src.real_time_detector import CrisisDetector

# Initialize detector
detector = CrisisDetector(model_path='models/best_model.pkl')

# Start real-time monitoring
detector.start_monitoring(location_filter=True, gps_radius=50)
```

### Interactive Map Interface
```python
from src.map_interface import CrisisMap

# Launch interactive map
crisis_map = CrisisMap()
crisis_map.launch_interface()
```

## Model Performance

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| Random Forest | 92.24% | 0.923 | 0.922 | 0.922 |
| SVM | 91.43% | 0.915 | 0.914 | 0.914 |
| XGBoost | 91.11% | 0.912 | 0.911 | 0.911 |
| Logistic Regression | 91.07% | 0.911 | 0.910 | 0.910 |

## Data Preprocessing

### Text Cleaning Pipeline
1. **Noise Removal**: Eliminates mentions, hashtags, URLs, and emojis
2. **Normalization**: Converts to lowercase and removes special characters
3. **Stopword Removal**: Filters out common non-informative words
4. **Lemmatization**: Reduces words to their base forms
5. **Parallel Processing**: Utilizes multiprocessing for efficient preprocessing

### Feature Engineering
- **TF-IDF Vectorization**: Converts text to numerical features
- **Dimensionality Reduction**: PCA for computational efficiency
- **Sentiment Analysis**: Polarity scores for enhanced classification
- **N-gram Analysis**: Bigram and trigram features for context

## API Endpoints

### Crisis Detection API
```
POST /api/detect-crisis
Content-Type: application/json

{
  "tweet": "Earthquake in downtown area, buildings shaking",
  "location": {"lat": 40.7128, "lng": -74.0060},
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Real-time Monitoring
```
GET /api/monitor/start
Parameters:
  - location: GPS coordinates (optional)
  - radius: Monitoring radius in km (default: 50)
  - keywords: Additional crisis keywords (optional)
```

## Configuration

### Environment Variables
```bash
# Twitter API credentials (if using Twitter API)
TWITTER_API_KEY=your_api_key
TWITTER_API_SECRET=your_api_secret
TWITTER_ACCESS_TOKEN=your_access_token
TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret

# Database configuration
DATABASE_URL=sqlite:///crisis_detection.db

# Model configuration
MODEL_PATH=models/crisis_model.pkl
CONFIDENCE_THRESHOLD=0.7
```

### Model Configuration
```python
# config.py
MODEL_CONFIG = {
    'random_forest': {
        'n_estimators': 100,
        'max_depth': 15,
        'min_samples_split': 5
    },
    'svm': {
        'C': 1.0,
        'kernel': 'rbf',
        'gamma': 'scale'
    },
    'xgboost': {
        'n_estimators': 100,
        'learning_rate': 0.1,
        'max_depth': 6
    }
}
```

## Deployment

### Docker Deployment
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 5000

CMD ["python", "app.py"]
```

### Cloud Deployment
The system is designed for cloud deployment with:
- Scalable architecture for high-volume tweet processing
- Load balancing for multiple model instances
- Database integration for historical data storage
- Real-time alerting system integration

## Monitoring and Alerts

### Performance Monitoring
- Model accuracy tracking
- Response time monitoring
- Error rate analysis
- Resource utilization metrics

### Alert System
- Real-time crisis notifications
- Geographic-based alert filtering
- Confidence level thresholds
- Integration with emergency services

## Contributing

### Development Setup
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Code formatting
black src/
flake8 src/
```

### Code Structure
```
├── src/
│   ├── data_preprocessing.py    # Data cleaning and preprocessing
│   ├── model_training.py        # Model training and evaluation
│   ├── real_time_detector.py    # Real-time crisis detection
│   ├── map_interface.py         # Interactive map functionality
│   └── utils.py                 # Utility functions
├── models/                      # Trained model files
├── data/                        # Dataset files
├── tests/                       # Unit tests
├── notebooks/                   # Jupyter notebooks for analysis
└── requirements.txt             # Python dependencies
```

## Limitations

- **Language Support**: Currently optimized for English tweets
- **Real-time Processing**: Dependent on Twitter API rate limits
- **Geographic Coverage**: GPS filtering requires location-enabled tweets
- **Model Bias**: Performance may vary across different types of crises

## Future Enhancements

- Multi-language support for global crisis detection
- Integration with additional social media platforms
- Advanced deep learning models (BERT, transformers)
- Real-time dashboard with advanced analytics
- Mobile application for crisis alerts
- Integration with emergency response systems

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Authors

- **Mehul Pahuja** - mehul22295@iiitd.ac.in
- **Adya Aggarwal** - adya22043@iiit.ac.in

## Acknowledgments

- Dataset contributors and open-source community
- IIIT Delhi for academic support
- Research papers and prior work in crisis detection
- Open-source libraries and frameworks used

## Support

For questions, issues, or contributions, please contact the development team or create an issue in the repository.

---

**Note**: This is a research project developed for educational purposes. For production deployment in emergency response systems, additional validation, testing, and integration with official emergency services would be required.
