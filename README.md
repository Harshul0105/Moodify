# 🎵 Moodify V3 — AI Music Recommendation from Photo

> Upload any photo. CLIP AI reads its mood. Get a live YouTube playlist — instantly.

---

## 🌟 What is Moodify?

**Moodify** is an AI-powered music recommendation app that analyses the emotional mood of any photo and returns a matching YouTube playlist in real time.

It uses **OpenAI's CLIP model** (zero-shot visual AI) to detect mood from your image — no training data, no fine-tuning needed. Just upload a photo, and the app figures out whether it feels *happy*, *mysterious*, *nostalgic*, *romantic*, or any of 18 distinct moods — then fetches live songs from YouTube that match that vibe.

Built as a Google Colab notebook with a clean **Gradio UI**.

---

## 🎬 Demo

| Step | What happens |
|------|-------------|
| 1 | Upload any photo (landscape, selfie, artwork — anything) |
| 2 | CLIP AI scores the image against 18 mood descriptions |
| 3 | Top mood is detected with a confidence breakdown |
| 4 | YouTube API fetches 6 live song recommendations |
| 5 | Clickable playlist displayed instantly in the UI |

---

## 🧠 How It Works

```
Your Photo
    │
    ▼
CLIP Model (openai/clip-vit-base-patch32)
    │  Compares image against 18 mood text descriptions
    │  Trained on 400M image-text pairs — zero-shot, no retraining needed
    ▼
Top Mood Detected (e.g. "peaceful 🌿" — 34.2% confidence)
    │
    ▼
YouTube Data API v3
    │  Searches: "peaceful relaxing calm ambient music"
    │  Filters for music category (videoCategoryId=10)
    ▼
Live Playlist — 6 clickable YouTube links
```

**CLIP zero-shot classification** works by embedding both the image and 18 text descriptions into the same vector space, then picking the text with the highest cosine similarity to the image. No labelled mood dataset required.

---

## ✨ Features

- **18 distinct moods** — happy, sad, angry, fearful, surprised, excited, peaceful, romantic, nostalgic, dreamy, hopeful, mysterious, intense, playful, awe, triumphant, horror, anticipation
- **Zero-shot AI** — uses OpenAI CLIP, no custom training data needed
- **Live YouTube recommendations** — real songs with direct links, fetched at query time
- **Confidence breakdown** — see top 5 mood scores with percentage confidence
- **Gradio UI** — clean browser interface, works entirely in Google Colab
- **Public share link** — Gradio generates a shareable URL automatically

---

## 🗂️ Project Structure

```
MoodifyV3.ipynb
├── Cell 1 — Install dependencies
├── Cell 2 — Configuration (API key, mood labels, colours, search queries)
├── Cell 3 — Import libraries
├── Cell 4 — Load CLIP model (openai/clip-vit-base-patch32)
├── Cell 5 — CLIP mood descriptions + predict_mood_clip()
├── Cell 6 — YouTube API → get_songs_from_youtube()
├── Cell 7 — Gradio prediction function → gradio_predict()
└── Cell 8 — Launch Gradio UI
```

---

## 🚀 Quick Start

### Option A — Google Colab (recommended)

1. Open the notebook in [Google Colab](https://colab.research.google.com/)
2. Get a free YouTube API key (instructions in Cell 2, takes ~10 minutes)
3. Paste your key into **Cell 2**
4. Click **Runtime → Run All**
5. A Gradio UI appears at the bottom — upload a photo and go

### Option B — Local (Jupyter)

```bash
# Clone the repo
git clone https://github.com/your-username/moodify.git
cd moodify

# Install dependencies
pip install torch torchvision transformers Pillow requests gradio

# Launch
jupyter notebook MoodifyV3.ipynb
```

---

## 🔑 YouTube API Setup (Free, ~10 minutes)

You need a free YouTube Data API v3 key to get live song recommendations.

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (e.g. `Moodify`)
3. Navigate to **APIs & Services → Enable APIs**
4. Search for **YouTube Data API v3** → click **Enable**
5. Go to **APIs & Services → Credentials → Create Credentials → API Key**
6. Copy the key and paste it into **Cell 2** of the notebook:

```python
YOUTUBE_API_KEY = 'paste-your-key-here'
```

> **Free tier:** YouTube Data API v3 gives 10,000 quota units/day for free. Each song search costs ~100 units, so you get ~100 free searches per day.

---

## 📦 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `torch` | ≥ 2.0 | PyTorch — runs CLIP model |
| `torchvision` | ≥ 0.15 | Image transforms |
| `transformers` | ≥ 4.30 | HuggingFace — loads CLIP |
| `Pillow` | ≥ 9.0 | Image loading & preprocessing |
| `requests` | ≥ 2.28 | YouTube API HTTP calls |
| `gradio` | ≥ 3.35 | Web UI |

All installed automatically in Cell 1.

---

## 🎭 Supported Moods

| Mood | Emoji | Search Query Style |
|------|-------|-------------------|
| Happy | 😊 | feel-good, upbeat, pop |
| Sad | 😢 | emotional, heartbreak, ballads |
| Angry | 😠 | aggressive rock, metal |
| Fearful | 😨 | dark, tense, suspenseful |
| Surprised | 😲 | uplifting, unexpected |
| Excited | 🤩 | high energy, party |
| Peaceful | 🌿 | ambient, calm, relaxing |
| Romantic | 💕 | love songs, R&B |
| Nostalgic | 🌅 | 80s/90s throwbacks |
| Dreamy | ✨ | ethereal, indie, lo-fi |
| Hopeful | 🌈 | uplifting, inspiring |
| Mysterious | 🌙 | atmospheric, dark ambient |
| Intense | 🔥 | dramatic, powerful |
| Playful | 🎈 | fun pop, kids |
| Awe | 🌌 | epic orchestral, cinematic |
| Triumphant | 🏆 | victory, epic anthems |
| Horror | 👻 | eerie, scary, dark |
| Anticipation | ⏳ | suspenseful, tension-building |

---

## 🔧 Configuration

All tunable settings live in **Cell 2**:

```python
YOUTUBE_API_KEY = 'your-key-here'   # YouTube Data API v3 key
N_SONGS = 6                          # Number of song recommendations per mood
```

To customise mood search queries, edit `MOOD_SEARCH_QUERIES` in Cell 2:

```python
MOOD_SEARCH_QUERIES = {
    'peaceful': 'peaceful relaxing calm ambient music',
    'happy':    'happy feel good upbeat songs playlist',
    # ... add or change any query
}
```

---

## 🧩 Tech Stack

- **[OpenAI CLIP](https://github.com/openai/CLIP)** (`clip-vit-base-patch32`) — Vision-Language model for zero-shot mood classification
- **[HuggingFace Transformers](https://huggingface.co/docs/transformers)** — Model loading and inference
- **[YouTube Data API v3](https://developers.google.com/youtube/v3)** — Live music search and retrieval
- **[Gradio](https://gradio.app/)** — Interactive web UI
- **[PyTorch](https://pytorch.org/)** — Deep learning inference backend

---

## ⚠️ Known Limitations

- **CLIP is a general model** — it wasn't trained specifically for mood detection, so some ambiguous images may get unexpected moods
- **YouTube API quota** — free tier is 10,000 units/day; heavy usage may hit the limit
- **First run is slow** — CLIP model download is ~600MB; cached after the first run
- **No GPU required** — runs on CPU (inference is fast for a single image), but GPU is faster
- **API key security** — do not commit your YouTube API key to a public repo; use environment variables or Colab secrets instead

---

## 🛡️ Security Note

**Never commit your API key to GitHub.** Instead, use:

```python
# In Colab — use Colab Secrets (left sidebar → 🔑 icon)
from google.colab import userdata
YOUTUBE_API_KEY = userdata.get('YOUTUBE_API_KEY')

# Locally — use environment variables
import os
YOUTUBE_API_KEY = os.environ.get('YOUTUBE_API_KEY')
```

---

## 🚧 Future Ideas

- [ ] Batch image upload — analyse multiple photos at once
- [ ] Spotify integration — richer song metadata + audio previews
- [ ] Mood history — track mood timeline across multiple uploads
- [ ] Custom mood vocabulary — let users define their own mood labels
- [ ] Mobile-friendly Gradio theme
- [ ] Export playlist to Spotify / YouTube Music

---

## 👤 Author

**Harshul Kumawat**
- GitHub: [@Harshul0105](https://github.com/Harshul0105)
- LinkedIn: [harshulkumawat-633665282](https://www.linkedin.com/in/harshulkumawat-633665282)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

*Built with CLIP + YouTube API + Gradio · JECRC University, Jaipur*
