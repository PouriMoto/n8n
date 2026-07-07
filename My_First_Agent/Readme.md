# n8n AI Agent Workflow – My First Agent

This repository contains a JSON workflow definition for **n8n**, a powerful workflow automation tool.  
The workflow implements a **Persian (Farsi) AI assistant** using a language model and a memory buffer. It is designed to be beginner‑friendly and is a great starting point for anyone who wants to build their first conversational agent with n8n.

---

## 🧠 What Does This Agent Do?

- Listens to chat messages (via the **Chat Trigger** node).
- Processes each message using an **AI Agent** node.
- Uses a **language model** (`qwen/qwen3-4b`) to generate responses.
- Retains conversation history with a **memory buffer**.
- Responds in **Persian** by default, but can switch languages if the user asks.
- Remembers the user’s name (if provided) and never confuses it with its own identity.

---

## 📁 File Overview

The file `my-first-agent.json` contains a complete n8n workflow with four nodes:

1. **When chat message received** – triggers the workflow on incoming messages.
2. **AI Agent** – the core logic that decides what to do with the input.
3. **OpenAI Chat Model** – the actual LLM (configured to use the `qwen/qwen3-4b` model via an OpenAI‑compatible API).
4. **Simple Memory** – stores the conversation history (last few exchanges) so the agent has context.

---

## 🚀 How to Import and Use It

### Prerequisites

- An **n8n instance** (self‑hosted, n8n.cloud, or any other).
- An **API key** for a service that provides the `qwen/qwen3-4b` model via an OpenAI‑compatible endpoint (e.g., Together AI, OpenRouter, or Qwen’s own API).  
  The workflow expects an n8n credential called **"OpenAI account"** – you can rename or update it later.

### Steps

1. **Download** the JSON file from this repository.

2. **Import** the workflow into n8n:
   - Go to your n8n dashboard.
   - Click **“Import from File”** (or paste the JSON directly).
   - Select the file.

3. **Set up the credential**:
   - The workflow uses a credential named `OpenAI account`.  
   - If you don’t have it, create a new **“OpenAI API”** credential in n8n and paste your API key.
   - If your API endpoint is different from the default OpenAI one, adjust the **“Base URL”** in the credential settings (for example, use `https://api.together.xyz/v1`).

4. **Activate** the workflow (toggle the switch to **Active**).

5. **Test it**:
   - Open the **Chat Trigger** node’s webhook URL (you can find it in the node’s settings) or use n8n’s built‑in chat interface.
   - Send a message in Persian, e.g., `سلام، اسم من احمد است`.
   - The agent should reply in Persian and remember your name.

---

## 🔧 Node‑by‑Node Explanation (for Beginners)

### 1. When chat message received
- **Type:** `@n8n/n8n-nodes-langchain.chatTrigger`
- This node creates a webhook that listens for incoming chat messages.  
- It’s the entry point of your workflow – every time a message arrives, the workflow starts.

### 2. AI Agent
- **Type:** `@n8n/n8n-nodes-langchain.agent`
- This is the “brain” of the assistant.  
- It receives the user’s message and decides which tools (if any) to use. In this simple version, it only uses the language model to generate a reply.
- The agent is configured with a **system message** (see below) that instructs it how to behave.

### 3. OpenAI Chat Model
- **Type:** `@n8n/n8n-nodes-langchain.lmChatOpenAi`
- This node connects to the actual LLM.  
- The model is set to `qwen/qwen3-4b`, but you can change it to any model your API supports.  
- The credential must be valid for the chosen API provider.

### 4. Simple Memory
- **Type:** `@n8n/n8n-nodes-langchain.memoryBufferWindow`
- Keeps a short history of the conversation (the last few messages).  
- Without memory, the agent would treat every message as a separate, isolated request.  
- This node gives the agent context, so it can refer to earlier parts of the conversation.

---

## 📝 System Prompt (What the Agent “Knows”)

The agent is given a clear set of rules via the `systemMessage`:

- **Role:** A helpful Persian AI assistant.
- **Identity:** Always distinguishes between the user and itself.
- **Name handling:** If the user says “My name is …”, the agent remembers that name and never uses it as its own.
- **Language:** Answers in Persian unless the user asks for a different language.
- **No reasoning:** It does not output its internal thinking process, and it never includes `<think>` tags.

You can modify this prompt directly in the **AI Agent** node to change the assistant’s personality or behaviour.

---

## ❓ Common Questions

**Can I change the model?**  
Yes – in the **OpenAI Chat Model** node, change the `model` parameter to any model available through your API.

**What if I don’t have access to `qwen/qwen3-4b`?**  
Use any other model (e.g., `gpt-3.5-turbo`, `llama-2-7b`, etc.) and update the credential accordingly.

**How do I see the conversation history?**  
The **Simple Memory** node stores it – you can increase the window size in its settings to remember more exchanges.

**Can I add tools (e.g., web search, calculator)?**  
Absolutely! This is a basic agent – you can extend it by connecting tool nodes (like HTTP Request, Code, or built‑in LangChain tools) to the AI Agent.

---

## 🤝 Contributing

This is a starter template. Feel free to fork, modify, and share your improvements. If you find issues or have suggestions, open an issue or a pull request.

---

## 📄 License

MIT – use it freely for learning, personal projects, or production.

---

Happy automating! 🚀
