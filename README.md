# General Knowledge Chatbot (Ilm)

A lightweight question-answering chatbot that uses TF-IDF vectorization and cosine similarity to find the closest matching question in a curated dataset of 500 general-knowledge Q&A pairs.

## Features

- 500 question-and-answer items in a text dataset
- TF-IDF text representation with unigrams and bigrams
- Cosine-similarity matching
- A similarity threshold for questions without a close match
- Gradio API endpoint named `/ask`
- Separate HTML/CSS/JavaScript frontend in `index.html`

## Project structure

```text
general-knowledge-chatbot/
├── app.py
├── requirements.txt
├── chatbot_general_qa_500.txt
├── index.html
├── README.md
└── notebooks/
    └── NetsolSession25.ipynb
```

## Run the backend locally

Python 3.10 or newer is recommended.

```bash
python -m venv .venv
```

Windows PowerShell:
```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```

macOS/Linux:
```bash
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

Open the local Gradio URL printed in the terminal. Keep `app.py`, `requirements.txt`, and `chatbot_general_qa_500.txt` together.

## Deploy the backend publicly with Hugging Face Spaces

1. Sign in to https://huggingface.co/ and create a new Space.
2. Choose **Gradio** as the Space SDK and make the Space public.
3. Upload `app.py`, `requirements.txt`, and `chatbot_general_qa_500.txt` to the Space root.
4. Wait for the Space to build. Open its URL and test a few questions.
5. The public Space URL will look like `https://YOUR-SPACE-NAME.hf.space` or be shown on the Space page.

The frontend's connection dialog accepts the Gradio Space URL. Open the hosted frontend, click **Not connected** (the status control), paste the public Space URL, and choose **Save and connect**.

## Deploy the frontend

### GitHub Pages
1. Push `index.html` to the root of a public GitHub repository.
2. Open repository **Settings → Pages**.
3. Under the build/deployment settings, select deployment from the `main` branch and `/ (root)`, then save.
4. GitHub will show the published URL. It commonly looks like `https://YOUR-USERNAME.github.io/general-knowledge-chatbot/`.

### Netlify
You can also import the GitHub repository in https://app.netlify.com/ and deploy the static `index.html` site. No build command is required for this plain HTML frontend.

## Frontend/backend connection

The frontend dynamically loads the Gradio JavaScript client and calls the API endpoint `/ask`. The backend in `app.py` sets `api_name="ask"` to match this. When connecting, paste the **public Hugging Face Space URL**, not a temporary local URL such as `127.0.0.1` or a `localhost` address.

## Limitations

- This is retrieval-based question matching, not a generative AI model. It selects an answer already present in the dataset.
- The chatbot may return an incorrect closest match for ambiguous questions. Adjust `SIMILARITY_THRESHOLD` in `app.py` after testing.
- The dataset is included in the public repository, so its contents will be publicly readable.
