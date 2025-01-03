<!-- srcbook:{"language":"typescript","tsconfig.json":{"compilerOptions":{"types":[],"strict":true,"module":"nodenext","moduleResolution":"nodenext","target":"es2022","resolveJsonModule":true,"noEmit":true,"allowImportingTsExtensions":true,"noPropertyAccessFromIndexSignature":true},"include":["src/**/*","env.d.ts"],"exclude":["node_modules"]}} -->

# LangChain Academy Basics

###### package.json

```json
{
  "type": "module",
  "dependencies": {
    "@langchain/openai": "^0.1.3",
    "@langchain/community": "^0.2.12",
    "@langchain/core": "^0.2.8",
    "tsx": "latest",
    "typescript": "latest",
    "@types/node": "latest"
  }
}
```

## Introduction to LangChain

LangChain aims to make building LLM applications easy, particularly agents that can automate a wide range of previously impossible tasks. LangGraph, a separate framework within the LangChain ecosystem, helps developers add better precision and control into agent workflows.

In this Srcbook, we'll explore the basics of working with Chat Models in LangChain, which take sequences of messages as inputs and return chat messages as outputs.

First, let's verify our environment is properly configured:

###### env-check.ts

```typescript
import assert from 'node:assert';

if (!process.env.OPENAI_API_KEY) {
    throw new Error("OPENAI_API_KEY environment variable is required");
}
if (!process.env.TAVILY_API_KEY) {
    throw new Error("TAVILY_API_KEY environment variable is required");
}
```

## Working with Chat Models

Let's explore how to use chat models with different configurations:

###### chat-models.ts

```typescript
import { ChatOpenAI } from "@langchain/openai";
import { HumanMessage } from "@langchain/core/messages";

// Initialize chat models with different configurations
const gpt4Chat = new ChatOpenAI({ 
    modelName: "gpt-4o", 
    temperature: 0 
});

const gpt35Chat = new ChatOpenAI({ 
    modelName: "gpt-3.5-turbo-0125", 
    temperature: 0 
});

// Create a message and invoke the model
const msg = new HumanMessage({ content: "Hello world" });

console.log("GPT-4 Response:");
const gpt4Response = await gpt4Chat.invoke([msg]);
console.log(gpt4Response);

console.log("\nGPT-3.5 Response:");
const gpt35Response = await gpt35Chat.invoke([msg]);
console.log(gpt35Response);

// You can also invoke with just a string
console.log("\nDirect string input:");
const stringResponse = await gpt4Chat.invoke("Tell me a short joke");
console.log(stringResponse);
```

## Using Search Tools

Tavily is a search engine optimized for LLMs and RAG. Let's see how to use it:

###### search-tools.ts

```typescript
import { TavilySearchResults } from "@langchain/community/tools/tavily_search";

const tavily = new TavilySearchResults({
    maxResults: 3
});

const searchResults = await tavily.invoke("What is LangGraph?");
console.log("Search Results:");
console.log(JSON.stringify(searchResults, null, 2));
```

## Understanding Messages

Messages in LangChain have roles and content properties. Here's how to work with them:

###### messages.ts

```typescript
import { HumanMessage, AIMessage, SystemMessage } from "@langchain/core/messages";
import { ChatOpenAI } from "@langchain/openai";

const chat = new ChatOpenAI({ 
    modelName: "gpt-4o", 
    temperature: 0 
});

// Create different types of messages
const systemMsg = new SystemMessage({
    content: "You are a helpful assistant who speaks in a friendly tone."
});

const humanMsg = new HumanMessage({
    content: "Can you explain what LangChain is?",
    name: "User"
});

// Send multiple messages in a conversation
const response = await chat.invoke([systemMsg, humanMsg]);
console.log("AI Response:", response);
```
