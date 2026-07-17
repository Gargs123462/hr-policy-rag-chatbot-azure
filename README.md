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
