## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework
## Name: Kanigavel M
## Reg.no: 212224240070
### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
To create a simple web-based chat application that lets users talk to an AI model in real time using a friendly interface.

### DESIGN STEPS:

#### STEP 1:
Set up all the necessary packages and API keys needed to run the model.

#### STEP 2:
Write a function to format the user’s messages and get responses from the AI model.

#### STEP 3:
Build a chat interface using Gradio with a textbox, a submit button, and a clear button.

### PROGRAM:
```

import os
from dotenv import load_dotenv, find_dotenv
import gradio as gr
from text_generation import Client

load_dotenv(find_dotenv())

HF_API_KEY = os.environ["HF_API_KEY"]
FALCON_ENDPOINT = os.environ["HF_API_FALCOM_BASE"]


client = Client(
    FALCON_ENDPOINT,
    headers={"Authorization": f"Basic {HF_API_KEY}"},
    timeout=120
)


def format_chat_prompt(message, chat_history):
    prompt = ""
    for user_msg, bot_msg in chat_history:
        prompt += f"User: {user_msg}\nAssistant: {bot_msg}\n"
    prompt += f"User: {message}\nAssistant:"
    return prompt

def respond(message, chat_history):
    prompt = format_chat_prompt(message, chat_history)

    # Generate using Falcon
    bot_message = client.generate(
        prompt,
        max_new_tokens=512,
        stop_sequences=["User:", "<|endoftext|>"]
    ).generated_text

    chat_history.append((message, bot_message))
    return "", chat_history

with gr.Blocks() as demo:
    gr.Markdown("## 🦅 Falcon LLM — Chatbot (Jupyter Notebook Version)")

    chatbot = gr.Chatbot(height=350)
    msg = gr.Textbox(label="Type your message...")
    submit = gr.Button("Send")
    clear = gr.ClearButton([msg, chatbot])

    submit.click(respond, [msg, chatbot], [msg, chatbot])
    msg.submit(respond, [msg, chatbot], [msg, chatbot])

demo.launch(share=False)   # share=True only if you want a public link
```

### OUTPUT:

<img width="1216" height="849" alt="Screenshot 2025-11-28 101428" src="https://github.com/user-attachments/assets/faeef50b-50ca-4ddc-9d49-12bf15cac38e" />


### RESULT:
A "Chat with LLM" application was successfully designed and deployed using the Gradio Blocks UI framework. The application provided an interactive interface that enabled smooth and dynamic communication between the user and the large language model.
