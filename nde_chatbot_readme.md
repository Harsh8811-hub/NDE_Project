# NDE Chatbot with PDF Memory, Voice Input, Web Scraping, and Image Generation

This repository contains a comprehensive AI-based chatbot designed for Non-Destructive Evaluation (NDE) tasks. It combines document retrieval from PDFs, voice-based interaction, generative AI capabilities using Google's Gemini API, and advanced image generation using multiple Stable Diffusion models.

The project is built to work efficiently on Google Colab and is modular, enabling easy customization.

---

## Table of Contents

- [Key Features](#key-features)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Modules Overview](#modules-overview)
- [PDF Text Extraction](#pdf-text-extraction)
- [Text Chunking and Embedding](#text-chunking-and-embedding)
- [Vector Search Using FAISS](#vector-search-using-faiss)
- [Chatbot with Memory](#chatbot-with-memory)
- [Voice-Based Chatbot](#voice-based-chatbot)
- [Text-to-Image Generator](#text-to-image-generator)
- [Web Scraping for Dataset Collection](#web-scraping-for-dataset-collection)

---

## Key Features

- **Textual Query + Memory**: Embeds NDE-related PDF data into vector space and retrieves relevant context for chat.
- **Gemini-Powered Responses**: Uses Google Gemini 2.5 Flash for high-quality, accurate answers.
- **Whisper-Based Voice Input**: Accepts questions via mic input and converts to text using Whisper.
- **Gradio Interface**: Launches interactive voice UI for real-time question-answering.
- **Image Generation**: Uses Stable Diffusion models to generate realistic visualizations of NDE equipment/scenarios.
- **Web Scraping**: Automated scraping of publicly available PDFs for dataset creation.

---

## Installation

Make sure you're using a Python environment like Google Colab or a system with GPU support.

Install all dependencies:

```bash
pip install sentence-transformers faiss-cpu google-generativeai PyMuPDF
pip install gradio openai-whisper
pip install whisper python-docx Document gradio faiss-cpu PyPDF2 pikepdf
pip install requests beautifulsoup4 googlesearch-python fpdf
```

---

## Getting Started

### 1. Configure Gemini API:

Replace `"<YOUR_API_KEY>"` with your own Gemini API Key:

```python
from google import genai
client = genai.Client(api_key="<YOUR_API_KEY>")
```

### 2. Upload and Extract Dataset:

Upload a zip file containing NDE-related PDFs and extract:

```python
zipfile.ZipFile("/content/dataset_NDE.zip").extractall("/content/nde_data")
```

### 3. Extract Text from PDFs:

Use PyMuPDF (`fitz`) to read and extract content page-by-page.

---

## Modules Overview

Each part of the system is modular and well-structured.

### PDF Text Extraction

Extracts all textual content from `.pdf` files:

- Uses PyMuPDF for parsing
- Automatically skips unreadable files

### Text Chunking and Embedding

- Breaks long text into smaller segments (\~300 chars)
- Uses `all-MiniLM-L6-v2` model from Sentence Transformers
- Converts all text chunks into dense vectors

### Vector Search Using FAISS

- Builds an index using `faiss.IndexFlatL2`
- Enables fast similarity search for retrieving relevant chunks given a query

---

## Chatbot with Memory

Includes memory-based retrieval from previous chats and context-aware prompts for Gemini.

- Chat history is stored in a list
- If user says "recall" or "previous question", the last interaction is shown

### Core Functions:

- `retrieve_context(query)` – Finds top-k similar chunks
- `ask_nde_bot(user_input)` – Combines retrieved context with user's query and sends it to Gemini
- `recall_last_question()` – Prints the last asked question and bot's answer

---

## Voice-Based Chatbot

Voice-based interaction via Whisper and Gradio:

- Uses `whisper.load_model("base")` for transcription
- User speaks a question, which gets transcribed and sent to Gemini
- Returns a natural response

**Gradio UI includes:**

- Audio input (mic)
- Text output box with bot response
- Real-time transcription display

---

## Text-to-Image Generator

Generates conceptual images using an ensemble of Stable Diffusion models:

- Models used: `runwayml/stable-diffusion-v1-5`, `dreamlike-art/dreamlike-photoreal-2.0`, `Realistic_Vision_V5.1_noVAE`
- All images blended using PIL’s `Image.blend()`
- Designed to visualize technical equipment like "ultrasonic testing machine" or "infrared thermography of composite material"

---

## Web Scraping for Dataset Collection

Scrapes PDFs from the web using:

- Google Search queries via `googlesearch`
- `requests` for downloading
- `BeautifulSoup` for parsing HTML
- `fitz` for reading downloaded PDFs
- Converts collected files into a single corpus

**Safety**: Includes retry logic, delay, and error handling (URLError, HTTPError, socket timeout).

---

## Notes

- All modules tested on Google Colab.
- GPU recommended for faster Whisper and Stable Diffusion performance.
- For custom prompts, you can modify the base input given to Gemini or Stable Diffusion.
- PDF extraction quality depends on scan quality and formatting.

---
