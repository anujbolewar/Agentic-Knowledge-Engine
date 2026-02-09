# Agentic Knowledge Engine (Agentic RAG MCP)

[![CI](https://github.com/anujbolewar/Agentic-Knowledge-Engine/actions/workflows/ci.yml/badge.svg)](https://github.com/anujbolewar/Agentic-Knowledge-Engine/actions/workflows/ci.yml)
[![Eval dashboard](https://img.shields.io/badge/evals-20%2F20%20passing-2DD4BF.svg)](https://anujbolewar.github.io/Agentic-Knowledge-Engine/)
[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![MCP](https://img.shields.io/badge/MCP-server-7C3AED.svg)](https://modelcontextprotocol.io)

A multi-agent Retrieval-Augmented Generation (RAG) system packaged as an MCP server.
It plans, retrieves evidence from a pgvector knowledge base, optionally augments with
live web research, drafts a cited answer, and self-critiques for grounding.

## Quickstart

```bash
uv venv && uv pip install -e ".[dev]"
cp .env.example .env
psql "$DATABASE_URL" -f sql/schema.sql
agentic-rag-mcp
```

## How it works

1. Plan - Claude turns the question into a plan plus 1-5 search queries.
2. Retrieve - each query is embedded and matched against pgvector by cosine distance.
3. Research - optional live web results appended (if FIRECRAWL_API_KEY is set).
4. Synthesize - Claude writes an answer grounded only in the numbered context.
5. Critique - a strict fact-check pass revises the answer until every claim is supported.

## Evaluation

Answer quality is measured with promptfoo against a golden dataset and enforced in CI.
Nothing is mocked; a regression fails the build before it ships.

## Deploy

Containerised for Railway (HTTP transport). Kubernetes manifests live in deploy/k8s/.

