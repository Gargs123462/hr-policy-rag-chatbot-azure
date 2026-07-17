# HR Policy RAG Chatbot (Azure AI Foundry + Search)

A retrieval-augmented generation (RAG) chatbot built on Azure AI Foundry, Azure AI Search, and Azure Blob Storage using Python. Answers HR policy questions using semantic search and grounded AI responses — no hallucinated answers, only what's in the documents.

**Stack:** Azure AI Foundry · Azure AI Search · Azure Blob Storage · Python · GPT-5-mini · text-embedding-3-small

## What It Does

An employee asks a question in plain English. The system searches indexed HR policy documents semantically, retrieves the most relevant chunks, and passes them to the language model as grounded context. The model answers using only what's in the documents, not general training knowledge.

## Architecture

Document Upload → Azure Blob Storage → Azure AI Search (indexing + embedding) → Query Input → Semantic Search → Retrieved Chunks → GPT-5-mini (grounded response) → Answer

## How It Was Built

1. Created Azure AI Foundry resource and project
2. Deployed GPT-5-mini as the chat model and text-embedding-3-small as the embedding model
3. Uploaded HR policy documents to Azure Blob Storage
4. Created Azure AI Search index with semantic configuration
5. Connected Blob Storage to AI Search for automatic document indexing
6. Wired semantic search to the chat model using Python SDK
7. Tested grounded responses against policy document content

## Real Debugging — What Actually Broke

**Model deprecation mid-build.** Started with gpt-4o-mini but it was unavailable in the deployment region. Pivoted to GPT-5-mini, which required finding the correct model string and redeploying. Not a clean swap — took real troubleshooting to identify the right available model.

**API version mismatches.** Azure AI Search and Azure OpenAI SDK versions had to be aligned manually. Default versions in the Python SDK didn't match what the Foundry deployment expected, causing silent failures on the search integration before the correct api-version parameter was identified and hardcoded.

**Index connection timing.** Blob Storage indexer runs on a schedule by default, not instantly. Documents uploaded to Blob didn't appear in the search index immediately, which looked like a configuration failure until the indexer run was triggered manually.

## Why This Matters

Most RAG implementations treat document retrieval as a black box. This build required understanding exactly how Azure AI Search chunks documents, generates embeddings, and scores semantic relevance — not just wiring a pre-built template together. The debugging required reading Azure SDK source documentation and matching API versions manually, the kind of problem-solving that shows up in real production deployments.

## Skills Demonstrated



Azure AI Foundry · Azure AI Search · Azure Blob Storage · RAG Architecture · Python SDK Integration · Semantic Search · Prompt Grounding · Real-World Debugging · Cloud-Native AI Deployment

<img width="2586" height="1798" alt="2" src="https://github.com/user-attachments/assets/4cee7232-d87a-429f-98f8-b18dc9ba3ab4" />
<img width="1954" height="1452" alt="6" src="https://github.com/user-attachments/assets/47111055-4eea-4a59-9903-86113e90a6a1" />
<img width="3160" height="1012" alt="5" src="https://github.com/user-attachments/assets/1eec12fb-3b39-4219-a151-9bba56e64a0b" />
<img width="2374" height="1592" alt="3" src="https://github.com/user-attachments/assets/847b0529-9895-498a-bca4-9758812b66b0" />
<img width="2626" height="1968" alt="4" src="https://github.com/user-attachments/assets/55a5155c-da97-42e6-87a9-764d4e53baeb" />
<img width="3796" height="1798" alt="1" src="https://github.com/user-attachments/assets/cfbaa95d-38dd-4985-8c44-ae1ab6955ea2" />

