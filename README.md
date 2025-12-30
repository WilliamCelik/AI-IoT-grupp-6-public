# 🌱 VäxtVärk AB - AI-Powered Agricultural Assistant

An intelligent conversational AI system for Swedish farmers and agricultural professionals, providing weather forecasts, weed identification, and herbicide recommendations through natural language interaction.

## 🎯 Project Overview

VäxtVärk AB combines AWS AI services (Lex, Rekognition) with real-time weather data (SMHI) to help farmers make informed decisions about weed management and crop protection. The system features a Swedish-language chatbot interface with image recognition capabilities for weed identification.

### Key Features

- **🤖 Conversational AI Assistant**: Natural language interaction in Swedish using Amazon Lex V2
- **🌤️ Real-Time Weather Data**: Integration with SMHI (Swedish Meteorological and Hydrological Institute) API for accurate local forecasts
- **📸 Visual Weed Recognition**: Custom-trained Amazon Rekognition model for identifying common Swedish weeds from images
- **💊 Herbicide Recommendations**: Database of effective treatments for identified weed species
- **📅 Smart Spray Day Prediction**: Analyzes 10-day forecasts to recommend optimal conditions for herbicide application
- **🌡️ Spray Condition Analysis**: Evaluates temperature, wind speed, humidity, and rainfall to determine treatment suitability

## 🏗️ Architecture

```
┌─────────────────┐
│   Streamlit     │  ◄── User Interface
│   Frontend      │
└────────┬────────┘
         │
         ├──────────────────────────┐
         │                          │
         ▼                          ▼
┌─────────────────┐      ┌──────────────────┐
│   Amazon Lex    │      │   API Gateway    │
│      Bot        │      │  (Image Upload)  │
└────────┬────────┘      └────────┬─────────┘
         │                        │
         └──────────┬─────────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │   AWS Lambda         │
         │   (Core Logic)       │
         └──────────┬───────────┘
                    │
         ┌──────────┼───────────────┐
         │          │               │
         ▼          ▼               ▼
┌──────────────┐ ┌───────────┐ ┌────────────┐
│   Amazon     │ │   SMHI    │ │  OpenCage  │
│ Rekognition  │ │ Weather   │ │  Geocoding │
│ (Custom      │ │    API    │ │     API    │
│  Model)      │ │           │ │            │
└──────────────┘ └───────────┘ └────────────┘
```

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- AWS Account with appropriate permissions
- AWS CLI configured
- Streamlit

### Installation

1. Clone the repository:
```bash
git clone https://github.com/WilliamCalk/AI-IoT-Grupp-6.git
cd AI-IoT-Grupp-6
```

2. Set up environment variables:
```bash
export LEX_BOT_ID="your_lex_bot_id"
export LEX_BOT_ALIAS_ID="your_bot_alias_id"
export AWS_REGION="eu-central-1"
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"
export LEX_LOCALE_ID="sv_SE"
```

3. Install Python dependencies:
```bash
pip install -r requirements.txt
```

4. Run the Streamlit application:
```bash
streamlit run streamlit_app/app.py
```

## 💬 Available Intents

### Weather Information (`Weatherinfo`)
Get detailed weather forecasts for any Swedish location.
- **Example**: "Vad blir det för väder i Stockholm imorgon?"
- **Returns**: Temperature, rainfall, wind speed, humidity, cloud cover, wind direction

### Weed Information (`Getweedinfo`)
Retrieve comprehensive information about specific weed species.
- **Example**: "Berätta om kvickrot"
- **Database**: JSON files with Swedish common names, Latin names, descriptions, and characteristics

### Herbicide Recommendations (`GetHerbicideInfo`)
Get treatment recommendations for identified weeds.
- **Example**: "Hur bekämpar jag maskros?"
- **Returns**: Effective herbicide products and application methods

### Best Spray Day Finder (`FindBestSprayDay`)
Analyze 10-day forecast to find optimal spraying conditions.
- **Example**: "När är det bäst att spruta i Göteborg?"
- **Criteria**: Temperature 10-25°C, wind speed < 5 m/s, no rain, humidity < 85%

### Spray Condition Check (`CheckSprayConditions`)
Evaluate if specific date meets spraying requirements.
- **Example**: "Går det att spruta i Uppsala på måndag?"
- **Analysis**: Detailed breakdown of weather factors

### Image Recognition (`/identify` endpoint)
Upload an image to identify weed species.
- **Model**: Custom Amazon Rekognition trained on Swedish weed dataset
- **Returns**: Species name with confidence score

## 🛠️ Technical Implementation

### Frontend (Streamlit)
- Interactive chat interface with message history
- Image upload component for weed identification
- Real-time conversation flow with AWS Lex
- Session management and state persistence

### Backend (AWS Lambda)
- **Handler Routing**: Distinguishes between Lex and API Gateway requests
- **Intent Processing**: Separate handlers for each conversation intent
- **External API Integration**: SMHI weather API and OpenCage geocoding
- **Error Handling**: Comprehensive logging and user-friendly error messages

### AI/ML Components
- **Amazon Lex V2**: Natural language understanding and conversation management
- **Amazon Rekognition**: Custom model trained on labeled weed images
- **Image Processing**: Base64 encoding for secure image transmission

## 📊 Data Sources

| Service | Purpose | API Endpoint |
|---------|---------|--------------|
| SMHI | Weather forecasts | `opendata-download-metfcst.smhi.se` |
| OpenCage | Geocoding (city → coordinates) | `api.opencagedata.com/geocode` |
| Amazon Lex | Conversational AI | AWS SDK |
| Amazon Rekognition | Image classification | AWS SDK |

## 🔒 Security Considerations

- API keys and credentials stored in AWS Secrets Manager (not in code)
- CORS enabled for secure cross-origin requests
- Session-based authentication
- Input validation on all user inputs
- No sensitive data in logs or public repositories

## 📈 Performance Metrics

- **Response Time**: < 2 seconds for weather queries
- **Image Recognition**: 1-3 seconds for weed identification
- **Accuracy**: Custom Rekognition model trained on 500+ labeled images
- **Availability**: Serverless architecture with automatic scaling

## 🧪 Testing

Run the Lambda function locally:
```bash
python -m pytest tests/
```

Test individual intents:
```bash
# Example: Test weather intent
aws lexv2-runtime recognize-text \
  --bot-id $LEX_BOT_ID \
  --bot-alias-id $LEX_BOT_ALIAS_ID \
  --locale-id sv_SE \
  --session-id test-session \
  --text "Vad blir det för väder i Stockholm imorgon?"
```

## 📚 Project Structure

```
AI-IoT-Grupp-6/
├── streamlit_app/
│   └── app.py              # Streamlit frontend application
├── lambda/
│   └── function.py         # AWS Lambda backend logic
├── requirements.txt        # Python dependencies
├── README.md              # Project documentation
├── /weeddata/             # JSON files with weed information
└── /README.md             # Setup and deployment instructions
```

## 🤝 Contributors

**Group 6 - VäxtVärk AB**
- Project developed as part of AI & IoT course
- Collaboration between agricultural domain experts and cloud engineers

## 📝 License

This project is for educational purposes. AWS services usage subject to AWS terms of service.

## 🔮 Future Enhancements

- [ ] Mobile app (iOS/Android) using AWS Amplify
- [ ] Multi-language support (English, German, Danish)
- [ ] Integration with farm management systems
- [ ] Historical weather data analysis
- [ ] Pest identification in addition to weeds
- [ ] IoT sensor integration for field conditions
- [ ] Push notifications for optimal spray windows

**Built with**: AWS Lambda, Lex, Rekognition | Python | Streamlit | SMHI API

**Last Updated**: April 2025
