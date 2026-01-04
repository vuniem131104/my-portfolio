---
title: Chatbot Recommendation System
description: Designed the core recommendation flow from retrieving products to ranking and filtering them by leveraging BM25, Embedding scores, and Graph Retrieval.
slug: recommendation-system
date: 2025-12-01 00:00:00+0000
image: cover.jpg
categories:
    - Company Projects
tags:
    - AI
    - LLM
    - Recommendation
weight: 1      
---

**Github Repository:** Private

# Graph-Based Recommendation System

<div align="center">

![Version](https://img.shields.io/badge/version-4.1.0-blue.svg)
![Python](https://img.shields.io/badge/python-3.11+-green.svg)

**Intelligent Chatbot Recommendation System powered by Graph Retrieval**

An enterprise-grade recommendation system that leverages graph databases, semantic search, and conversational AI to provide context-aware product recommendations.

</div>

---

## Table of Contents

- [Overview](#-overview)
- [How It Works](#-how-it-works)
- [Architecture](#-architecture)
- [Key Technologies](#-key-technologies)
- [Recommendation Flow](#-recommendation-flow)
- [Features](#-features)

---

## Overview

The **Sun Assistant Recommendation System** is designed to provide intelligent product recommendations through natural language conversations. By combining **Graph Retrieval**, **Semantic Search**, and **Conversational AI**, the system delivers accurate, context-aware recommendations that understand user intent and product relationships.

### Core Capabilities

- **Hybrid Search** - Combines BM25 keyword matching with semantic embedding scores
- **Graph Retrieval** - Leverages Neo4j to discover product relationships and connections
- **Conversational Interface** - Natural language interaction through chatbot
- **Intelligent Ranking** - Multi-factor scoring including relevance, popularity, and context
- **Context Awareness** - Maintains conversation history for better recommendations

---

## How It Works

The recommendation system follows a sophisticated multi-stage pipeline:

### 1. **Query Understanding**
The chatbot processes user queries using LLM (Large Language Models) to:
- Extract user intent and preferences
- Identify key entities and attributes
- Decompose complex questions into sub-queries
- Maintain conversation context

### 2. **Multi-Source Retrieval**
The system retrieves candidate products from three complementary sources:

**Vector Search (OpenSearch)**
- Semantic similarity using embeddings
- Captures meaning beyond exact keywords
- Returns products with similar descriptions

**Keyword Search (BM25)**
- Traditional text matching
- Effective for exact product names or specifications
- Handles specific attribute queries

**Graph Retrieval (Neo4j)**
- Discovers product relationships
- Finds similar or complementary products
- Traverses connections like "bought together", "similar to", "alternative for"
- Explores product hierarchies and categories

### 3. **Ranking & Filtering**
Retrieved candidates are scored and filtered using:
- **Relevance Score**: Combination of BM25 and embedding similarity
- **Graph Score**: Relationship strength from Neo4j
- **Business Rules**: Availability, price range, categories
- **Context Filtering**: User preferences and conversation history

### 4. **Answer Generation**
The chatbot synthesizes recommendations into natural language:
- Explains why products are recommended
- Highlights key features and benefits
- Provides comparison insights
- Asks clarifying questions when needed

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    User Query (NL)                       │
│              "Show me running shoes under $100"          │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
          ┌──────────────────────┐
          │   Chatbot Service    │
          │  (Query Processing)  │
          │   • Intent Extraction │
          │   • Sub-questions    │
          │   • Context Manager  │
          └──────────┬───────────┘
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
┌──────────┐  ┌──────────┐  ┌──────────┐
│OpenSearch│  │  Neo4j   │  │Azure AI  │
│          │  │  Graph   │  │ Search   │
│ Semantic │  │ Relation │  │ BM25     │
│ Vectors  │  │ Traversal│  │ Keywords │
└────┬─────┘  └────┬─────┘  └────┬─────┘
     │            │            │
     └────────────┼────────────┘
                  │
                  ▼
        ┌─────────────────────┐
        │  Ranking Engine     │
        │  • Score Fusion     │
        │  • Filter Rules     │
        │  • Context Weight   │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │  Answer Generator   │
        │  (LLM Synthesis)    │
        │  • Format Results   │
        │  • Add Explanations │
        │  • Generate Response│
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │   Natural Language  │
        │      Response       │
        └─────────────────────┘
```

---

## Key Technologies

### Graph Database (Neo4j)
**Why Graph for Recommendations?**

Graph databases excel at representing and querying relationships:
- **Product Relationships**: "similar_to", "complementary", "bought_together"
- **Category Hierarchies**: Navigate from specific items to broader categories
- **User Preferences**: Track user interests and interaction patterns
- **Fast Traversals**: Efficiently explore multi-hop connections

**Example Graph Query:**
```cypher
// Find products similar to running shoes
MATCH (p:Product {name: "Nike Air Max"})-[:SIMILAR_TO]->(similar:Product)
WHERE similar.price < 100
RETURN similar.name, similar.price, similar.rating
ORDER BY similar.rating DESC
LIMIT 5
```

### Vector Search (OpenSearch)
- Stores product embeddings (768-dimensional vectors)
- Performs semantic similarity search
- Captures meaning beyond exact text matches
- Fast approximate nearest neighbor (ANN) search

### Hybrid Scoring
Combines multiple signals for optimal results:

$$
\text{FinalScore} = \alpha \cdot \text{BM25Score} + \beta \cdot \text{EmbeddingScore} + \gamma \cdot \text{GraphScore}
$$

Where:
- $\alpha, \beta, \gamma$ are tunable weights
- BM25Score: Keyword relevance
- EmbeddingScore: Semantic similarity
- GraphScore: Relationship strength

---

## Recommendation Flow

### Example Conversation

**User:** "I need running shoes for trail running"

**System Process:**

1. **Query Analysis**
   - Intent: Product search
   - Category: Shoes → Running Shoes → Trail Running
   - Constraints: None explicit

2. **Retrieval**
   - **Vector Search**: Products with similar descriptions to "trail running shoes"
   - **BM25**: Exact matches for "trail running" and "shoes"
   - **Graph**: 
     ```
     Trail Running → requires → Trail Running Shoes
     Trail Running Shoes → similar_to → Hiking Shoes
     Trail Running Shoes → complementary → Running Socks
     ```

3. **Ranking**
   - Top 10 trail running shoes by combined score
   - Filter by availability and ratings
   - Boost popular brands

4. **Response**
   ```
   I found 5 great trail running shoes for you:
   
   1. Salomon Speedcross 5 ($130) ⭐ 4.8/5
      - Excellent grip for technical trails
      - Waterproof design
      
   2. Brooks Cascadia 16 ($140) ⭐ 4.7/5
      - Comfortable for long distances
      - Good cushioning
   
   Would you like more details or have a specific price range?
   ```

**User:** "Something under $100?"

**System Process:**

1. **Context Update**: Add price constraint < $100
2. **Re-rank**: Apply price filter to previous results
3. **Graph Expansion**: Look for alternatives in lower price range
4. **Response**: Updated recommendations within budget

---

## Features

### Intelligent Query Understanding
- Multi-turn conversation with context retention
- Intent classification and entity extraction
- Handles ambiguous queries with clarifying questions
- Understands synonyms and variations

### Multi-Modal Retrieval
- **Semantic Search**: Understands meaning and context
- **Keyword Matching**: Precise for specific terms
- **Graph Traversal**: Discovers related products through relationships

### Advanced Ranking
- **Relevance**: How well product matches query
- **Popularity**: User ratings and purchase frequency
- **Freshness**: Recent additions and trending items
- **Diversity**: Varied results to increase discovery

### Graph-Powered Insights
- "Customers who bought this also bought..."
- "Similar products you might like"
- "Complete your set with..."
- Product comparison and alternatives

### Personalization (Future)
- User preference learning
- Interaction history tracking
- Collaborative filtering via graph

---

## Benefits

### For Users
- Natural language interaction - no complex filters needed
- Discover products through relationships and connections
- Contextual recommendations that understand intent
- Explanations for why products are recommended

### For Business
- Better product discovery → higher conversion
- Relationship-based recommendations → cross-selling
- Understand product connections and gaps
- Scalable architecture for large catalogs

### Technical Advantages
- **Flexible**: Easy to add new relationship types
- **Explainable**: Graph paths show recommendation reasoning
- **Scalable**: Distributed search and graph processing
- **Accurate**: Hybrid approach combines best of multiple methods

---

## Performance

### Retrieval Speed
- Vector Search: < 50ms (top 100 candidates)
- Graph Traversal: < 100ms (2-3 hop queries)
- BM25 Search: < 30ms
- Total Recommendation Time: < 500ms

### Accuracy Metrics
- **Precision@5**: Relevance of top 5 results
- **Recall@20**: Coverage of relevant products
- **MRR** (Mean Reciprocal Rank): Position of first relevant result
- **Graph Coverage**: % of products connected in graph

---

## Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Chatbot Engine** | LangGraph, FastAPI | Conversation orchestration |
| **LLM** | Azure OpenAI, Claude | Query understanding & generation |
| **Vector DB** | OpenSearch | Semantic search |
| **Graph DB** | Neo4j 5.22 | Relationship retrieval |
| **Search** | Azure AI Search | BM25 keyword matching |
| **Backend** | Python 3.11+, FastAPI | API services |
| **Embeddings** | Azure OpenAI Ada-002 | Text to vectors |

---

## Key Learnings

### Why Graph Retrieval?
Traditional recommendation systems rely on:
- Content-based filtering (similar features)
- Collaborative filtering (similar users)

**Graph retrieval adds:**
- Explicit relationship modeling
- Multi-hop reasoning
- Explainable connections
- Dynamic relationship discovery

### Hybrid is Better
No single retrieval method is perfect:
- Vectors capture semantics but miss exact matches
- Keywords find exact terms but miss meaning
- Graphs discover relationships but need initial candidates

**Combining all three provides the best results.**

---

<div align="center">

**Built by Sun Asterisk**

*Intelligent recommendations through conversational AI and graph intelligence*

</div>
