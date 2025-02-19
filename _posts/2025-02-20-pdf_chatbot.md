---
title: Create a chatbot for your pdf
date: 2025-02-20
categories: [easy, guide]
tags: []
description: A simple guide on how to load, process, and ask questions about your pdf using langchain.
---

# PDF ChatBot
This is a guide to read in a pdf, hand it off to langchain for tokenization & RAG processing, then use llama3.2 for Q/A

## Dependencies
```bash
pip install langgraph langchain-ollama langchain-community faiss-cpu PyPDF2
```

## The LLM

For our purposes, we will use Llama3.2, but you can use something much more sophisticated like Llama3.3 or even Deepseek R1. 

```bash
ollama pull llama3.2
```

```python
from langchain_ollama.llms import OllamaLLM
llm = OllamaLLM(model="llama3.2")
```

## Downloading the pdf

Here I download the Australian Constitution, but you can easily modify it to suit your needs like loading in a folder of PDFs.

```python
from PyPDF2 import PdfReader
import urllib

urllib.request.urlretrieve("https://www.aph.gov.au/-/media/05_About_Parliament/52_Sen/523_PPP/2023_Australian_Constitution.pdf", "main.pdf")

reader = PdfReader("main.pdf")
text = ""
for page in reader.pages:
    text += page.extract_text()

print(f"Loaded {text.count('\n')} lines")
```

## Creating the vector database

This part just goes though our loaded text and splits it into multiple chunks that we can later use as context for our llm. Play around with the chunk size and overlap if you are not getting the results you want.

```python
from langchain_ollama import OllamaEmbeddings
from langchain.vectorstores import FAISS
from langchain.text_splitter import CharacterTextSplitter

text_splitter = CharacterTextSplitter(
    separator = "\n",
    chunk_size = 512,
    chunk_overlap = 256,
    length_function = len
)

chunks = text_splitter.split_text(text)
embeddings = OllamaEmbeddings(model="llama3.2")

vectorstore = FAISS.from_texts(texts = chunks, embedding = embeddings)
```

## Creating the history retriever

Normally, we would use the user's question to query the database, but it is better for the chatbot to create the query themselves from the provided chat history.

Here we use llama to achieve this conversion, in combination with a custom prompt so it understands its task better.

```python
from langchain.chains.history_aware_retriever import create_history_aware_retriever
from langchain_core.prompts import MessagesPlaceholder
from langchain_core.prompts.chat import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    MessagesPlaceholder(variable_name="chat_history"),
    ("user", "{input}"),
    ("user", "Given the above conversation, generate a search query to look up in order to get information relevant to the conversation")
])

history_retriever_chain = create_history_aware_retriever(llm, vectorstore.as_retriever(), prompt)
```

## Create the document chain

Separately from the history retriever, we need to actually instruct langchain how the user / chatbot / context is expected to be formatted in the llm's prompt.

```python
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents.stuff import create_stuff_documents_chain

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a chatbot that attempts to answer the user's questions. You will be provided context that can help answer the user's query. Here is the provided context:\n\n{context}"),
    MessagesPlaceholder(variable_name="chat_history"),
    ("user", "{input}")
])

document_chain = create_stuff_documents_chain(llm, prompt)
conversational_retrieval_chain = create_retrieval_chain(history_retriever_chain, document_chain)
```

## Start chatting

The response also contains what documents / sections were referenced, but here we just throw it away for the sake of simplicity.

```python
from langchain_core.messages.human import HumanMessage
from langchain_core.messages.ai import AIMessage

print("Type 'exit' to stop")
chat_history = []

while True:
    user_input = input(" >")
    if user_input == 'exit':
        break
    
    print(user_input)
    
    response = conversational_retrieval_chain.invoke({
        'chat_history': chat_history,
        "input": user_input
    })

    print(response['answer'])
    chat_history.append(AIMessage(response['answer']))
```

### Question
Is there anything in the Australian Constitution that prevents a large language model from becoming the prime minister? 


### Answer
The Australian Constitution does not explicitly state that the Prime Minister must be human. Article 61 of the Constitution states that "All political power within the Commonwealth, whether derived from the People or granted by Parliament, shall be vested in a Federal Senate and a House of Representatives."

There is no provision in the Constitution that excludes non-human entities or artificial intelligence from holding public office. However, it's worth noting that Article 61 also provides for the appointment of senators to represent each state, which are typically human beings.

Additionally, Article 64 states that "The Governor-General shall appoint such Ministers as he thinks fit, subject to, and in accordance with this Constitution." This provision does not explicitly exclude non-human entities from being appointed ministers.

It's worth noting that while there is no constitutional barrier to a large language model becoming Prime Minister, it would likely face significant practical challenges. For example, the ability of a large language model to communicate effectively with the public, understand and represent their interests, and make informed decisions on policy matters would be a subject of ongoing debate.

In summary, while the Australian Constitution does not explicitly exclude non-human entities from holding public office, it is likely that a large language model would face significant practical challenges in assuming the role of Prime Minister.

