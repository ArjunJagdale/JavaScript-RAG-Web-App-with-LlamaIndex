---

# 🦙 LlamaIndex: Production-Ready Techniques with Deno + TypeScript

This project demonstrates production-level techniques using [LlamaIndex](https://github.com/jerryjliu/llama_index) with Deno and TypeScript. You'll learn how to persist data, interact via queries and chat, implement streaming responses, and deploy it as a web app—all while leveraging the `llamaindex` npm package.

> This project was a part of a course by DeepLearning.ai, I've done this project in deeplearning.ai's environment and hence only the video based demo can be shown.
---

### 🧩 How RAG Works Here
Retrieval:

Documents are embedded and stored as a vector index using VectorStoreIndex.

When a user query is received, relevant document chunks are fetched based on semantic similarity (Top K).

Augmentation:

Retrieved text is injected into the prompt context and passed to the LLM.

Generation:

The LLM (powered by LlamaIndex’s default backend) generates responses conditioned on both the query and retrieved documents.

Chat Engine:

For contextual conversations, ContextChatEngine maintains message history and uses the retriever to fetch relevant information for follow-ups.

### 📜 Course Completion

I successfully completed the **"Production-Ready Retrieval-Augmented Generation"** course by [DeepLearning.AI](https://www.deeplearning.ai/) on their GenAI learning path.

🏅 [View Certificate](https://learn.deeplearning.ai/accomplishments/14d5cf63-385c-49c9-bf7f-44f7fb5ee680?usp=sharing)

---

### Demo
The first question about Dan's major achievements was not answered correctly, as no context was provided.
The second question, however, was answered well—it focused on what he did in college.
The third question about React and Redux received a detailed and thorough response.

https://github.com/user-attachments/assets/0352d37a-197b-4892-a329-7425e9b26d0d

## 📦 Setup

First, install dependencies and load your `.env` file:

```ts
import * as mod from "https://deno.land/std@0.213.0/dotenv/mod.ts";
import {
    Document,
    VectorStoreIndex,
    SimpleDirectoryReader,
    RouterQueryEngine,
    storageContextFromDefaults,
    ContextChatEngine
} from "npm:llamaindex@0.1.12"

const keys = await mod.load({ export: true }); // Load API key from .env
```

---

## 1. 📚 Persisting Your Data

### 🗂️ Set Up a Storage Context

```ts
const storageContext = await storageContextFromDefaults({
  persistDir: "./storage",
});
```

### 📄 Load the Data and Create an Index

```ts
const documents = await new SimpleDirectoryReader().loadData({
  directoryPath: "./data", // Example: React Wikipedia page
});

let index = await VectorStoreIndex.fromDocuments(documents, {
  storageContext
});
```

### ❓ Ask It a Question

```ts
let engine = await index.asQueryEngine();
let response = await engine.query({ query: "What is JSX?" });
console.log(response.toString());
```

---

### 💾 Load Index Without Re-Parsing Documents

```ts
let storageContext2 = await storageContextFromDefaults({
    persistDir: "./storage",
});

let index2 = await VectorStoreIndex.init({
    storageContext: storageContext2
});
```

### 🔍 Query the Loaded Index

```ts
let engine2 = await index2.asQueryEngine();
let response2 = await engine2.query({ query: "What is JSX?" });
console.log(response2.toString());
```

---

## 2. 💬 Chatting With Your Data

### 🔁 Create a Retriever and a Chat Engine

```ts
const retriever = index2.asRetriever();
retriever.similarityTopK = 3;

let chatEngine = new ContextChatEngine({ retriever });
```

### 💡 Try a Chat Session

```ts
let messageHistory = [
  { role: "user", content: "What is JSX?" },
  {
    role: "assistant",
    content: "JSX stands for JavaScript Syntax Extension. It is an extension to the JavaScript language syntax that provides a way to structure component rendering using syntax familiar to many developers..."
  }
];

let newMessage = "What was that last thing you mentioned?";

const response3 = await chatEngine.chat({
  message: newMessage,
  chatHistory: messageHistory
});
console.log(response3.toString());
```

---

## 3. 🔄 Streaming Responses

### 📡 Enable Streaming Mode

```ts
const response4 = await chatEngine.chat({
  message: newMessage,
  chatHistory: messageHistory,
  stream: true,
});
```

### 🖨️ Check the Streamed Output

```ts
for await (const data of response4) {
  console.log(data.response);
}
```

---

## 4. 🌐 Create a Web App

### 🧩 Run Llama App

You can demonstrate all the techniques in a live web app using utility functions.

```ts
import { runCreateLlamaApp } from './utils.ts';

runCreateLlamaApp();
```

> 📝 **Note**: This might take a minute. Once done, click the generated link to access your web app!

### 🌟 UI Demo

Once deployed, you can ask it questions (e.g., about React), and observe real-time streamed responses.

---


## 🚀 Tech Stack

* [Deno](https://deno.land/)
* [LlamaIndex (JS)](https://www.npmjs.com/package/llamaindex)
* Vector Search & RAG
* Context-Aware Chat
* Streaming Chat Responses

---
