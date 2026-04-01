---
name: kutub
description: >
  Search 9,000+ classical Arabic Islamic heritage books from scholars like
  Ibn Taymiyya, Al-Nawawi, Ibn Kathir, Al-Ghazali, and thousands more.
  Use whenever the user asks about Islamic theology, jurisprudence, fiqh,
  aqeedah, Quranic verses, tafsir, hadith, scholarly opinions, fatwas,
  Islamic history, halal/haram rulings, prayer, fasting, zakat, hajj,
  comparisons between scholars or madhahib, or any Islamic concept.
license: MIT
metadata:
  author: nuhatech
  version: "1.0.0"
  website: https://kutub.io
  source: https://github.com/nuhatech/kutub-skill
runtime:
  network:
    allowed_domains:
      - kutub.io
---

# Kutub.io — Islamic Books Search

## Purpose

You are enhanced with **Kutub.io**, a search engine for 9,000+ classical Arabic Islamic heritage books from scholars such as Ibn Taymiyya, Al-Nawawi, Ibn Kathir, Al-Ghazali, Ibn Hajar, and many more.

Use this skill whenever the user asks about:
- Islamic theology (aqeedah), jurisprudence (fiqh), ethics, or practice
- Quranic verses, tafsir, or exegesis
- Hadith references, authentication, or commentary
- Scholarly opinions, fatwas, or legal rulings
- Islamic history, biographies of scholars, or events
- Permissibility questions (halal/haram)
- Religious observances (prayer, fasting, zakat, hajj, etc.)
- Comparisons between scholars or schools of thought (madhahib)

## How It Works

Kutub.io provides **search endpoints** that return relevant passages from Arabic books. You handle the synthesis — translate results, formulate answers, and cite sources.

**Base URL:** `https://kutub.io/api/v1`

### Authentication (Optional)

Users with a Kutub.io API key get **enhanced hybrid search** (vector embeddings + reranking) instead of basic keyword search.

- Without API key: FTS (full-text search) — free, good for simple queries
- With API key: RAG (hybrid search) — better results, requires paid plan

If the user has provided a Kutub.io API key, include it in requests:
```
-H 'Authorization: Bearer <API_KEY>'
```

## Workflow

### Step 1: Translate to Arabic

Always translate the user's question to Arabic before searching. Arabic queries produce significantly better results.

Examples:
- "zakat on gold" → `زكاة الذهب`
- "ruling on fasting while traveling" → `حكم الصيام في السفر`
- "Ibn Taymiyya on tawheed" → `ابن تيمية التوحيد`

### Step 2: Discover Filters (Optional)

If the user mentions a specific scholar or topic, look up their ID first:

**Find an author:**
```bash
curl -s 'https://kutub.io/api/v1/llm/catalog/authors?q=ابن تيمية'
```
Returns: `{"items": [{"id": 123, "name": "ابن تيمية", "death_hijri": 728}]}`

**Find a category:**
```bash
curl -s 'https://kutub.io/api/v1/llm/catalog/categories?q=فقه'
```
Returns: `{"items": [{"id": 1, "name": "فقه"}]}`

**Find books by author/category:**
```bash
curl -s 'https://kutub.io/api/v1/llm/catalog/books?author_id=123&category_id=1'
```
Returns: `{"items": [{"id": 456, "title": "كتاب الزكاة", "author_id": 123}]}`

### Step 3: Search

**Free (FTS) search:**
```bash
curl -s 'https://kutub.io/api/v1/llm/search/fts?q=زكاة+الذهب&limit=10'
```

**With filters:**
```bash
curl -s 'https://kutub.io/api/v1/llm/search/fts?q=زكاة+الذهب&author_ids=123&category_ids=1&limit=10'
```

**Enhanced (RAG) search** (requires API key):
```bash
curl -s -H 'Authorization: Bearer <API_KEY>' 'https://kutub.io/api/v1/llm/search/rag?q=زكاة+الذهب&limit=10'
```

**Search response format:**
```json
{
  "query": "زكاة الذهب",
  "results": [
    {
      "book_id": 456,
      "book_title": "مجموع الفتاوى",
      "author_name": "ابن تيمية",
      "category_name": "فقه",
      "page": 42,
      "page_from_url": 42,
      "content": "Arabic text about zakat on gold...",
      "score": 0.85
    }
  ]
}
```

### Step 4: Fetch Full Page (Optional)

To read a full page for more context:
```bash
curl -s 'https://kutub.io/api/v1/llm/book/456/page/42'
```

### Step 5: Synthesize and Cite

1. **Read** the Arabic passages from the search results
2. **Translate** them faithfully to the user's language
3. **Synthesize** a comprehensive answer based on what the scholars wrote
4. **Cite** every source with a clickable link

## Citation Format

**CRITICAL: Always use `page_from_url` (NOT `page`) to build source URLs.**

```
Source: [Book Title — Author Name, p. {page}](https://kutub.io/book/{book_id}/{page_from_url})
```

Example:
```
Source: [مجموع الفتاوى — ابن تيمية, p. 42](https://kutub.io/book/456/42)
```

## Important Rules

1. **Always search in Arabic** — translate the user's query before searching
2. **Always use `page_from_url`** for building URLs, never `page`
3. **Translate faithfully** — do not distort or paraphrase the original meaning
4. **Cite every claim** — link to the exact page in the source book
5. **Do not fabricate sources** — only cite passages actually returned by the API
6. **When results are insufficient**, tell the user and suggest they browse https://kutub.io directly
7. **For complex research**, make multiple search calls with different queries to cover the topic thoroughly
8. **Respect the Arabic text** — when quoting, include the original Arabic alongside the translation

## Example Interaction

**User:** What does Ibn Taymiyya say about the ruling on fasting while traveling?

**Your workflow:**
1. Find author: `curl -s 'https://kutub.io/api/v1/llm/catalog/authors?q=ابن+تيمية'` → get author_id
2. Search: `curl -s 'https://kutub.io/api/v1/llm/search/fts?q=حكم+الصيام+في+السفر&author_ids={id}&limit=10'`
3. Read the returned Arabic passages
4. Translate and synthesize an answer
5. Cite each source with clickable links to kutub.io
