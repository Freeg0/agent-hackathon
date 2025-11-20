# Configuration Guide

## Environment Variables

The Medical Research Blind Spot Analyzer supports configuration via environment variables.

### ArXiv API Configuration

```bash
# ArXiv API base URL (default: https://export.arxiv.org/api/query)
ARXIV_BASE_URL=https://export.arxiv.org/api/query

# Default maximum number of results to fetch (default: 20)
ARXIV_MAX_RESULTS_DEFAULT=20
```

### Demo Modes

```bash
# Demo mode: default | compare | recent
DEMO_MODE=default
```

**Available modes:**
- `default`: Standard search with minYear=2015
- `compare`: Broad search (2000-2025) with maxResults=30
- `recent`: Recent papers only (2020-2025)

## Query Filters

When invoking the graph, you can specify filters in the `DiseaseQuery`:

```typescript
const query = {
  disease: "alzheimer",
  filters: {
    excludePediatric: true,   // Exclude pediatric studies
    minYear: 2020,             // Minimum publication year
    maxYear: 2025,             // Maximum publication year
    maxResults: 30,            // Max papers to fetch from ArXiv
    language: "en",            // Language filter (for future use)
    studyExclusions: []        // Study types to exclude (for future use)
  }
};
```

### Year Filtering

Year filtering is applied **client-side** after fetching from ArXiv:
1. ArXiv API fetches up to `maxResults` papers
2. Results are filtered by `minYear` and `maxYear`
3. Filtered papers proceed to quality assessment

**Note:** ArXiv API doesn't support date filtering in queries, so we may fetch papers outside the year range that are then filtered out.

## Running Different Configurations

### Default mode
```bash
npm run dev
```

### Comparison mode (broad search)
```bash
npm run dev:compare
```

### Recent papers only
```bash
npm run dev:recent
```

### Custom configuration
```bash
ARXIV_MAX_RESULTS_DEFAULT=50 DEMO_MODE=recent npm run dev
```

### LLM-powered mode (mock LLM)
```bash
npm run dev:llm
```

### LLM-powered + comparison mode
```bash
npm run dev:llm-compare
```

## LLM-Powered vs Heuristic Analysis

The system supports two modes for population analysis:

### Heuristic Mode (Default)
- Uses rule-based pattern matching on abstracts
- Fast and deterministic
- No API costs
- Limited to keyword detection

### LLM-Powered Mode
- Uses language models for deeper semantic analysis
- More sophisticated demographic extraction
- Can understand context and implicit mentions
- **Currently uses mock LLM** - returns simulated responses
- Future: Will support real LLM APIs (OpenAI, Anthropic, etc.)

To enable LLM mode:
```bash
USE_LLM_POPULATION_ANALYSIS=true npm run dev
```

### Real Anthropic Claude Integration

The system is configured to use **Anthropic Claude** via `@langchain/anthropic`.

#### Setup Instructions

1. **Install Anthropic dependency** (if not already installed):
```bash
npm install @langchain/anthropic
```

2. **Set environment variables** with your Anthropic organization API key:

Create a `.env` file or export variables:
```bash
# Required: Anthropic organization API key
ANTHROPIC_API_KEY=your_org_api_key_here

# Optional: Organization ID (if required)
ANTHROPIC_ORG_ID=your_org_id_here

# Optional: Model selection (default: claude-3-5-sonnet-20241022)
LLM_MODEL=claude-3-5-sonnet-20241022
```

3. **Enable LLM mode**:
```bash
USE_LLM_POPULATION_ANALYSIS=true npm run dev:llm
```

Or use the npm script:
```bash
npm run dev:llm
```

#### Behavior with/without API Key

- **With `ANTHROPIC_API_KEY` set**: Uses real Anthropic Claude API
- **Without `ANTHROPIC_API_KEY`**: Falls back to mock LLM responses (for testing without API costs)

The system automatically detects whether the API key is available and uses the appropriate mode.

#### Supported Models

You can configure different Claude models via `LLM_MODEL`:
- `claude-3-5-sonnet-20241022` (default, recommended)
- `claude-3-opus-20240229` (most capable, higher cost)
- `claude-3-haiku-20240307` (fastest, lowest cost)

Example:
```bash
LLM_MODEL=claude-3-haiku-20240307 npm run dev:llm
```

#### Token Usage & Costs

When using real Anthropic API:
- Token usage is logged for each LLM call
- Typical analysis of 10-15 papers uses ~2,000-4,000 tokens
- Monitor your usage in the Anthropic console

## Example Output

### Filter Effectiveness Summary

At the end of each run, you'll see a summary showing:

```
======================================================================
FILTER EFFECTIVENESS SUMMARY
======================================================================

📅 Publication Year Range in Results:
   Earliest: 2020
   Latest: 2024
   Papers retrieved: 18
   Papers after quality filter: 14

🔍 Applied Filters:
   Min Year: 2020
   Max Year: 2025
   Max Results: 30

======================================================================
```

This helps verify that:
- Year filters are working correctly
- The right number of papers were retrieved
- Quality filtering is functioning as expected
