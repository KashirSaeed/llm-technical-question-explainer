# Technical Question Explainer

A simple LLM-powered technical Q&A tool that takes a programming or technical question and generates an explanation using two different language models:

* **OpenAI GPT-4o-mini** — cloud-based LLM
* **Llama 3.2 via Ollama** — locally running LLM

The project demonstrates how to integrate both cloud and local LLMs, send the same technical question to both models, and stream their responses in real time.

---

## Project Overview

This project was developed as part of an **LLM Engineering Week 1 exercise**.

The goal is to build a reusable technical question-answering tool that can help explain:

* Python code
* Programming concepts
* Algorithms
* Data structures
* Software engineering concepts
* Technical terminology

For example, you can provide:

```text
Please explain what this code does and why:

yield from {book.get("author") for book in books if book.get("author")}
```

The question is sent to both GPT-4o-mini and Llama 3.2, allowing you to see how different LLMs explain the same technical problem.

---

## Architecture

```text
                    Technical Question
                           │
                           ▼
                  ┌─────────────────┐
                  │ Python Program  │
                  └────────┬────────┘
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
      OpenAI GPT-4o-mini          Ollama Llama 3.2
        Cloud LLM                    Local LLM
              │                         │
              ▼                         ▼
        Streaming Response       Streaming Response
              │                         │
              └────────────┬────────────┘
                           ▼
                  Technical Explanation
```

---

## Key Features

* Technical question input
* OpenAI API integration
* Local LLM integration using Ollama
* GPT-4o-mini support
* Llama 3.2 support
* Streaming responses
* Same question sent to two different models
* Simple Python implementation
* Useful for learning and debugging code

---

## Technologies

* Python
* OpenAI API
* Ollama
* GPT-4o-mini
* Llama 3.2
* Jupyter Notebook

---

## Project Structure

```text
technical-question-explainer/
│
├── technical_question_explainer.ipynb
├── requirements.txt
├── .gitignore
└── README.md
```

### Files

**`technical_question_explainer.ipynb`**

Contains the Python implementation for sending technical questions to OpenAI and Ollama.

**`requirements.txt`**

Contains the Python packages required to run the project.

**`.gitignore`**

Prevents sensitive files, Python cache files, virtual environments, and temporary files from being committed.

**`README.md`**

Project documentation and setup instructions.

---

# Requirements

Before running the project, install:

* Python 3.10+
* OpenAI API key
* Ollama
* Llama 3.2 model
* Jupyter Notebook or JupyterLab

---

# Installation

## 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/technical-question-explainer.git
```

Move into the project directory:

```bash
cd technical-question-explainer
```

---

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

### Linux/macOS

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

---

## 3. Install Python Dependencies

Run:

```bash
pip install -r requirements.txt
```

The required packages are:

```text
openai
ollama
python-dotenv
jupyter
```

---

# OpenAI Configuration

The OpenAI model requires an API key.

Create a file named:

```text
.env
```

in the project root directory.

Add:

```text
OPENAI_API_KEY=your_api_key_here
```

Do not replace this with your actual key in the README.

The Python application can load the key through the environment.

### Important Security Rule

**Never commit your `.env` file to GitHub.**

Your `.gitignore` should contain:

```text
.env
```

Never hardcode an API key directly into the notebook.

Avoid:

```python
api_key = "sk-xxxxxxxxxxxxxxxx"
```

Use an environment variable instead.

---

# Ollama Setup

Ollama is used to run Llama 3.2 locally.

Install Ollama on your computer and then verify the installation:

```bash
ollama --version
```

Pull the Llama 3.2 model:

```bash
ollama pull llama3.2
```

Verify that the model is available:

```bash
ollama list
```

You should see `llama3.2` in the list.

You can also test the model directly:

```bash
ollama run llama3.2
```

---

# Running the Project

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open:

```text
technical_question_explainer.ipynb
```

Run the notebook cells from top to bottom.

---

# Asking Your Own Technical Question

Find the `question` variable:

```python
question = """
Please explain what this code does and why:
yield from {book.get("author") for book in books if book.get("author")}
"""
```

Replace it with your own question.

For example:

```python
question = """
Explain the difference between a Python list and a tuple.
Give a simple example of when I should use each one.
"""
```

Or:

```python
question = """
Explain how a Python dictionary works internally.
Keep the explanation beginner-friendly.
"""
```

Or:

```python
question = """
Explain what this code does:

numbers = [1, 2, 3, 4, 5]
result = [x * 2 for x in numbers]

Why is list comprehension useful?
"""
```

Run the notebook again to receive explanations from both models.

---

# How the Code Works

## 1. Import Libraries

```python
from openai import OpenAI
import ollama
```

`OpenAI` provides access to the OpenAI API.

`ollama` provides access to locally running Ollama models.

---

## 2. Define Models

```python
MODEL_GPT = "gpt-4o-mini"
MODEL_LLAMA = "llama3.2"
```

These constants define which models will answer the question.

---

## 3. Create the OpenAI Client

```python
openai = OpenAI()
```

The OpenAI client uses the configured API credentials to communicate with the OpenAI API.

---

## 4. Define the Question

```python
question = """
Please explain what this code does and why:
yield from {book.get("author") for book in books if book.get("author")}
"""
```

This is the technical question that will be sent to both models.

You can replace this question with any technical question you want to explore.

---

# OpenAI Response

The question is sent to GPT-4o-mini:

```python
response = openai.chat.completions.create(
    model=MODEL_GPT,
    messages=[
        {
            "role": "user",
            "content": question
        }
    ],
    stream=True
)
```

The important part is:

```python
stream=True
```

This enables streaming.

Instead of waiting for the entire answer to be generated, the application receives the response progressively.

---

# Streaming the OpenAI Response

```python
for chunk in response:
    content = chunk.choices[0].delta.content

    if content:
        print(content, end="")
```

The program receives small pieces of the response and prints them immediately.

This creates a more interactive experience.

---

# Ollama Response

The same question is then sent to Llama 3.2:

```python
response = ollama.chat(
    model=MODEL_LLAMA,
    messages=[
        {
            "role": "user",
            "content": question
        }
    ],
    stream=True
)
```

Again:

```python
stream=True
```

allows the response to be displayed progressively.

The response is printed using:

```python
for chunk in response:
    print(chunk["message"]["content"], end="")
```

---

# Cloud LLM vs Local LLM

This project demonstrates two different approaches to using LLMs.

| OpenAI GPT-4o-mini           | Ollama Llama 3.2                     |
| ---------------------------- | ------------------------------------ |
| Cloud-based                  | Runs locally                         |
| Requires API access          | Runs through Ollama                  |
| API usage may have costs     | No API call cost for local inference |
| Requires internet access     | Can run locally after model download |
| Managed model infrastructure | User manages local model/runtime     |

The two models can produce different explanations for the same question.

This makes the project useful for experimenting with LLM behavior and model comparison.

---

# What Is Streaming?

Without streaming, an application may wait for the complete response before displaying anything.

With streaming:

```text
Model generates response
        ↓
First tokens arrive
        ↓
Display immediately
        ↓
More tokens arrive
        ↓
Continue displaying
```

This improves perceived responsiveness for interactive applications.

---

# Example

### Input

```text
Explain what this code does:

yield from {book.get("author") for book in books if book.get("author")}
```

### GPT-4o-mini

The model provides an explanation of:

* Set comprehension
* Dictionary `.get()`
* Filtering
* `yield from`
* Generator behavior

### Llama 3.2

The local model receives the same question and produces its own explanation.

The exact responses may vary between models and between executions.

---

# Why This Project Is Useful

This small project demonstrates several important concepts used in modern LLM applications:

* Calling an LLM API
* Running a local LLM
* Prompt construction
* Model selection
* Streaming responses
* Environment variable management
* Comparing cloud and local inference
* Building reusable AI developer tools

---

# Security

Never commit secrets to GitHub.

Do not commit:

```text
.env
```

Do not put your actual API key inside:

```python
technical_question_explainer.ipynb
```

Use environment variables instead.

If an API key is accidentally exposed publicly, revoke it and create a new one.

---

# Limitations

This project is intentionally simple and designed as a learning exercise.

Current limitations include:

* Questions are entered directly in the notebook.
* No web interface.
* No conversation history.
* No persistent chat history.
* No automated evaluation of model responses.
* No structured response format.
* Model responses can vary in quality.
* Ollama requires sufficient local hardware resources for the selected model.

---

# Future Improvements

Possible extensions include:

* Build a Streamlit web interface
* Add a command-line interface
* Add conversation history
* Support multiple Ollama models
* Allow model selection
* Compare response latency
* Compare response length
* Add automated response evaluation
* Add logging
* Add error handling
* Add configurable system prompts
* Add RAG for documentation-based questions
* Add code execution in a controlled environment

---

# Learning Outcomes

After completing this project, you should understand:

* How to call an OpenAI model from Python
* How to communicate with an Ollama model
* The difference between cloud and local LLM inference
* How streaming responses work
* How to construct basic LLM messages
* How to manage API credentials securely
* How to build a simple reusable LLM developer tool

---

# License

This project is intended for educational and portfolio purposes.
