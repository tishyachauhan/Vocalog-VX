# Vocalog-VX

**Vocalog VX** (Voice & Chat Intelligence) is a privacy-first, fully offline pipeline for extracting insight from **customer conversations** — whether they come from **call center recordings** or **chatbot logs**.

It transforms raw audio and chat interactions into structured summaries, topic highlights, and sentiment analysis — all without sending any data to the cloud.

---

## Why Vocalog VX?

### Unified:
Handles **both audio and text** seamlessly — from voice calls (`.wav`, `.mp3`, etc.) to JSON/txt chatbot exports.

### Private by Design:
Runs **completely offline**, with no internet or external API calls. Ideal for sensitive environments (e.g., finance, healthcare, enterprise support).

### Reliable & Lightweight:
Optimized for local execution — no GPU needed, no external infrastructure.

### Insightful:
Extracts:
- Speaker diarized transcripts
- Summarized conversation flow
- Key topics and terms
- Sentiment label and confidence

---

## Key Use Cases
- QA audits of call center agents
- Customer satisfaction analysis
- Support ticket enrichment
- Training data generation for AI/chatbots
- Internal tools for CX and analytics teams




---

## 📂 Supported Input File Types

- `.wav`, `.mp3`, `.m4a`, `.flac` → Audio recordings
- `.txt`, `.json` → Pre-transcribed conversations

---

## Core Workflow Steps

```
                 +----------------+
                 |  Upload File   |
                 +--------+-------+
                          |
      +-------------------+--------------------+
      |                                        |
+-----v-----+                          +--------v--------+
|  Audio    |                          |    Text/JSON     |
| Preprocess|                          | Conversation     |
+-----+-----+                          +--------+--------+
      |                                         |
+-----v-----+                           +-------v--------+
| Diarize   |                           | Format JSON     |
| Speakers  |                           +----------------+
+-----+-----+
      |
+-----v-----+
| Transcribe|   ---> Whisper (ASR)
+-----+-----+
      |
+-----v-----+
| Format    |   ---> JSON conversation with user/agent turns
+-----+-----+
      |
+-----v-----+
| Summarize |   ---> BART (philschmid/bart-large-cnn-samsum)
+-----+-----+
      |
+-----v-----+
| Extract   |   ---> KeyBERT (top keywords/topics)
| Topic     |
+-----+-----+
      |
+-----v-----+
| Sentiment |   ---> RoBERTa (twitter-roberta-base-sentiment)
| Analysis  |
+-----------+
```

---

## Models Used

| Task                  | Model/Tool Used                                  |
|-----------------------|--------------------------------------------------|
| Transcription (ASR)   | `openai/whisper`                                 |
| Diarization           | `Resemblyzer` + `KMeans`                         |
| Summarization         | `philschmid/bart-large-cnn-samsum` (HuggingFace) |
| Topic Extraction      | `KeyBERT`                                        |
| Sentiment Analysis    | `cardiffnlp/twitter-roberta-base-sentiment`      |

---

## ⚙️ How to Use

### 1. Install Requirements
Create a virtual environment (optional but recommended), then install dependencies:

```bash
pip install -r requirements.txt
```

### 2. Run the Pipeline

```python
from main import process_scrap_call_pipeline

# Pass in a dictionary with the file path as the key
result = process_scrap_call_pipeline({"sample_call.wav": None})
print(result)
```

### Example Output:
```json
{
  "call_duration_seconds": 102.45,
  "topic": "Pickup scheduling issue",
  "sentiment": "positive",
  "sentiment_score": 0.73,
  "summary": "The agent helped the user schedule a scrap pickup..."
}
```

---

## 🗂️ Project Structure (To be Modified in future: Minimal)

```
sentiment_audio_project/
├── main.py                      # Entry point with pipeline logic
├── models/
│   ├── whisper_model.py         # Whisper ASR logic
│   ├── diarizer.py              # Resemblyzer + KMeans
│   ├── summarizer.py            # BART summarization
│   ├── keyword_model.py         # KeyBERT topic extractor
│   └── sentiment_model.py       # RoBERTa sentiment analysis
├── requirements.txt             # All dependencies
├── README.md                    # Project overview and usage
```

---

## Summary

This pipeline enables offline, fast and automated analysis of call center audio and chat data. With support for both audio and text input, it produces:
- A clean summary
- Conversation topic
- Sentiment label and score

Ready for integration into analytics dashboards, customer experience tools, or CRM systems.

---

Need help deploying this or integrating it into a web UI or Flask API? Let me know.
