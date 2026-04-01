# Kutub.io Skill

Search **9,000+ classical Arabic Islamic books** from within your AI assistant. Works with Claude, Copilot, Cursor, Gemini CLI, and more.

Find passages from scholars like Ibn Taymiyya, Al-Nawawi, Ibn Kathir, Al-Ghazali, and thousands more — with proper citations and source links.

## Installation

### Claude.ai / Claude Desktop

1. Download this repository as a ZIP
2. Go to **Settings > Customize > Skills**
3. Upload the ZIP file

### Claude Code

```bash
# Clone and copy the skill directory
git clone https://github.com/nuhatech/kutub-skill.git
cp -r kutub-skill ~/.claude/skills/kutub
```

Or install with npx:
```bash
npx openskills install nuhatech/kutub-skill
```

### GitHub Copilot

```bash
cp -r kutub-skill .github/skills/kutub
```

### Cursor

```bash
cp -r kutub-skill .cursor/skills/kutub
```

### Gemini CLI

```bash
cp -r kutub-skill ~/.gemini/skills/kutub
```

## Usage

Once installed, just ask your AI assistant questions about Islamic topics:

- *"What is the ruling on zakat for gold?"*
- *"What does Ibn Taymiyya say about fasting while traveling?"*
- *"Compare Al-Nawawi and Ibn Hajar on the conditions of prayer."*
- *"Find hadith about patience in Sahih Muslim."*

The skill will automatically search Kutub.io's database and provide sourced answers with clickable links.

## Enhanced Search (Optional)

By default, the skill uses **free full-text search**. For better results with **hybrid search** (vector embeddings + semantic matching + reranking), you can add a Kutub.io API key:

1. Create an account at [kutub.io](https://kutub.io)
2. Subscribe to a paid plan
3. Generate an API key at [kutub.io/profile (API Keys tab)](https://kutub.io/profile (API Keys tab))
4. Provide the key to your AI assistant when prompted

## What's Included

| Capability | Free | With API Key |
|---|---|---|
| Full-text keyword search | Yes | Yes |
| Hybrid RAG search (vector + reranking) | - | Yes |
| Browse authors, categories, books | Yes | Yes |
| Fetch specific book pages | Yes | Yes |
| Citation links to kutub.io | Yes | Yes |

## API Endpoints Used

| Endpoint | Auth | Description |
|---|---|---|
| `GET /api/v1/llm/search/fts` | None | Full-text keyword search |
| `GET /api/v1/llm/search/rag` | API key | Hybrid RAG search |
| `GET /api/v1/llm/catalog/authors` | None | Browse/search authors |
| `GET /api/v1/llm/catalog/categories` | None | Browse/search categories |
| `GET /api/v1/llm/catalog/books` | None | Browse/search books |
| `GET /api/v1/llm/book/{id}/page/{page}` | None | Fetch a specific page |

## License

MIT License — see [LICENSE](LICENSE).
