## 🐼 PuBaO: A Low-Latency Voice-Based Self-Exploration System

PuBaO is an **AI-powered voice interaction system** designed to support emotional reflection and structured self-exploration through natural conversations.

The system combines **real-time speech processing, an empathy-first LLM workflow, and a structured counseling framework** to provide an accessible conversational experience.

### 1. Motivation

| As-Is: Conventional LLM Interfaces | To-Be: PuBaO |
|---|---|
| Text-based interaction | Natural voice-based interaction |
| Waiting for complete LLM responses disrupts the flow of emotionally sensitive conversations. | A low-latency pipeline enables immediate empathetic responses while separately assessing conversational progress. |

### 2. Key Features
* **Real-Time Voice Interaction**: Integrates speech recognition, LLM response generation, and speech synthesis.
* **Empathy-First LLM Workflow**: Separates immediate response generation from answer-sufficiency assessment and follow-up question selection.
* **Structured Self-Exploration**: Incorporates Satir's Iceberg Model and personal value exploration to guide conversations.
* **Interactive 3D Environment**: Provides spatial onboarding before users enter the counseling interface.

### 3. System Architecture

The system consists of a Next.js frontend, a FastAPI backend, and a LangGraph-based counseling pipeline.

* **Voice interaction pipeline:** User Speech → Silero VAD → whisper.cpp STT → Gemini-Based Counseling Pipeline → GPT-SoVITS TTS → AudioWorklet Playback

* **Counseling pipeline:** User Input → Empathetic Response Generation → Answer-Sufficiency Assessment → Conditional Dialogue Routing → Next Question or Follow-Up

### 4. Methodology

* **Voice Processing**

Silero VAD detects speech boundaries, while pre-speech buffering and silence detection help preserve utterances and avoid unnecessary transcription requests.
Completed speech segments are transcribed using whisper.cpp. The resulting text is passed to the counseling pipeline.
For speech synthesis, GPT-SoVITS generates audio that is delivered through Raw PCM streaming and played continuously using AudioWorklet.

* **Dual-Role LLM Architecture**

The LangGraph-based counseling pipeline separates two responsibilities:
* Response Generator: Produces an empathetic response with minimal reasoning overhead.
* Dialogue Analyzer: Evaluates whether the user's response provides sufficient information for the current conversation stage.
Conditional routing determines whether to proceed to the next stage or generate a follow-up question. This separation supports responsive interactions while maintaining structured dialogue progression.

* **Counseling Framework**

PuBaO draws on Satir's Iceberg Model to explore emotional and cognitive dimensions such as feelings, perceptions, expectations, yearnings, and the self.
The conversational flow includes rapport building, personal value exploration, guided conversation, and report generation.

### 5. Experimental Results

The project presentation reports an **average response generation time of 1.44 seconds** for the empathy-first LLM workflow. This result was reported during the project demo day rather than independently reproduced as a benchmark.

### 6. Installation & Usage

#### Prerequisites
Python 3.10+, Node.js 20+, Gemini API credentials, Required local speech models and services for voice functionality

#### Backend
```
python3 -m venv .venv
source .venv/bin/activate
pip install -r backend/requirements-dev.txt

cp backend/.env.example backend/.env

cd backend
../.venv/bin/python -m uvicorn app.server:app --host 127.0.0.1 --port 8000
```

#### Frontend

Open a separate terminal:
```
cd frontend
cp .env.example .env.local
npm ci
npm run dev
```
Frontend (Counseling Screen): http://127.0.0.1:3000

Backend: http://127.0.0.1:8000

### 7. My Contributions

**Minsuh Joo: AI & Backend**

* **LLM Architecture**: Designed the LangGraph-based counseling workflow, structuring empathetic response generation, answer-sufficiency assessment, and conditional dialogue routing.

* **Speech-to-Text (STT)**: Implemented the speech recognition pipeline using Silero VAD and whisper.cpp, incorporating speech-boundary detection and pre-speech buffering for reliable transcription.

