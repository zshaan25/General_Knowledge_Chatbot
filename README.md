# General Knowledge Chatbot

An interactive, browser-based General Knowledge Chatbot that answers questions using a dataset of **500 question-and-answer facts**. The chatbot uses Python, TF-IDF vectorization, and cosine similarity to identify the closest matching question and return its stored answer.

The backend runs in **Google Colab using Gradio**, while the custom `index.html` frontend provides an interactive chat experience in the browser.

## Features

- Answers general-knowledge questions using 500 stored facts.
- Uses TF-IDF vectorization with one-word and two-word n-grams.
- Uses cosine similarity to identify the closest matching question.
- Returns a fallback message when no suitable match is found.
- Provides an interactive browser-based chat interface.
- Connects the frontend to the Gradio backend using a public URL.
- Includes questions covering Pakistan, geography, science, mathematics, history, computing, sports, religion, and everyday knowledge.

## Technologies Used

- **Python** — chatbot logic and data processing
- **Google Colab** — running the Python notebook
- **Gradio** — exposing the chatbot through an API
- **scikit-learn** — TF-IDF vectorization and cosine similarity
- **HTML, CSS, JavaScript** — frontend development

## Project Structure

```text
general-knowledge-chatbot/
├── index.html
├── chatbot_general_qa_500.txt
├── NetsolSession25.ipynb
└── README.md
```

*Note: The notebook may be stored inside a `notebooks/` directory if that is how the repository is organized.*

## How to Run the Project

The project has two main components:

1. **Backend:** The Python chatbot runs in Google Colab and is exposed through Gradio.
2. **Frontend:** The `index.html` file runs in your browser and communicates with the Gradio backend.

### Step 1: Open the Google Colab Notebook

1. Open `NetsolSession25.ipynb` in Google Colab.
2. Upload `chatbot_general_qa_500.txt` to the Colab session.
3. Ensure the dataset-loading cell uses the correct path:

   ```python
   file_path = "/content/chatbot_general_qa_500.txt"
   ```

4. Run the cells that load the dataset, extract the questions and answers, and create the TF-IDF vectorizer and question vectors.

**Important:** Do not run the old `while True` input loop. The Gradio interface will handle user questions instead.

### Step 2: Launch the Gradio Backend

After running the dataset-loading and TF-IDF cells, add a new code cell at the end of the notebook and run:

```python
!pip install -q gradio

import gradio as gr
from sklearn.metrics.pairwise import cosine_similarity

def chatbot(user_question):
    user_vector = vectorizer.transform([user_question])

    scores = cosine_similarity(
        user_vector, questions_vector
    ).flatten()

    best_index = scores.argmax()
    best_score = scores[best_index]

    if best_score < 0.2:
        return "Sorry, I do not know the answer to that."

    return answers[best_index]

def ask(user_question):
    user_question = (user_question or "").strip()

    if not user_question:
        return "Please type a question."

    return chatbot(user_question)

demo = gr.Interface(
    fn=ask,
    inputs=gr.Textbox(
        label="Your question",
        placeholder="e.g. What is the capital of Pakistan?"
    ),
    outputs=gr.Textbox(label="Answer"),
    title="General Knowledge Chatbot",
    description="Ask questions from a dataset of 500 facts.",
    api_name="ask",
    flagging_mode="never",
)

demo.launch(share=True)
```

Wait for Gradio to display a public URL similar to:

```text
https://xxxxxxxx.gradio.live
```

Copy this URL. It is the address the frontend will use to communicate with your chatbot.

**Keep the Colab cell running.** The backend will stop responding if the cell stops, the Colab runtime disconnects, or the temporary share link expires.

### Step 3: Open the Frontend

1. Download or locate `index.html` from this repository.
2. Double-click the file to open it in Google Chrome, Microsoft Edge, or another modern browser.
3. Find the connection or status control near the top of the page.
4. Click the control to open the backend connection settings.
5. Paste the public Gradio URL copied from Colab.
6. Click **Save and connect**.
7. Return to the chat interface and ask a question.

You can test the chatbot with questions such as:

- What is the capital of Pakistan?
- Who founded Pakistan?
- What is the largest planet?
- How many bones are there in the adult human body?

The frontend uses the Gradio API endpoint named `ask`.

### Step 4: Troubleshooting

**The frontend cannot connect to Gradio**

- Make sure the Gradio cell is still running in Colab.
- Check that you pasted the complete public URL.
- Ensure the backend was launched with `api_name="ask"`.

**The Gradio URL has expired**

- Run the Gradio cell again.
- Copy the newly generated public URL.
- Paste the new URL into the frontend connection settings.

**The chatbot returns a fallback answer**

- Try rephrasing the question.
- Use wording closer to the questions stored in the dataset.
- Remember that this chatbot retrieves existing answers rather than generating new ones.

**The website works on your computer but not for other users**

- Opening `index.html` directly provides a local frontend.
- To make the frontend publicly accessible, host the HTML file on a static website hosting service.
- The backend must also be reachable through a valid public Gradio URL.

## How It Works

1. The notebook loads and parses the 500 question-and-answer pairs.
2. TF-IDF transforms the stored questions and the user's question into numerical vectors.
3. Cosine similarity measures how closely the user's question matches each stored question.
4. The chatbot selects the question with the highest similarity score.
5. If the best score is below the threshold of `0.2`, the chatbot returns a fallback message.
6. Otherwise, the corresponding stored answer is returned through Gradio and displayed in the browser.

## Dataset

The `chatbot_general_qa_500.txt` file contains **500 question-and-answer items**.

Each item follows a consistent format containing a numbered entry, a `Q:` line, and an `A:` line. The dataset covers:

- Pakistan
- World geography
- Science and mathematics
- Animals and the human body
- History and literature
- Computing
- Sports and religion
- Inventions and everyday knowledge

## Limitations

- The chatbot retrieves answers from a fixed dataset; it is not a generative AI model.
- Its knowledge is limited to the facts included in the dataset.
- Similarity matching can return an imperfect answer when a question is ambiguous or phrased differently.
- The temporary Gradio share URL may expire and is intended for demonstrations and testing rather than guaranteed permanent hosting.
- The Google Colab runtime must remain active for the backend to respond.

## Future Improvements

- Expand the dataset with additional questions and answers.
- Improve typo handling and question matching.
- Add automated tests and more robust error handling.
- Host the frontend and backend using reliable services for more consistent public access.
