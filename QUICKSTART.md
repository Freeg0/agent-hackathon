# Quick Start Guide

## Installation

```bash
# Install dependencies
npm install
```

## Basic Usage

### 1. Run with Heuristic Analysis (Default)

No API key required - uses rule-based pattern matching:

```bash
npm run dev
```

### 2. Run with Anthropic Claude (LLM-Powered)

Requires Anthropic organization API key:

```bash
# Set your API key
export ANTHROPIC_API_KEY=your_org_api_key_here

# Run with LLM mode
npm run dev:llm
```

Or create a `.env` file:

```bash
# .env
ANTHROPIC_API_KEY=your_org_api_key_here
USE_LLM_POPULATION_ANALYSIS=true
```

Then run:
```bash
npm run dev
```

## Available Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Default mode (heuristic analysis) |
| `npm run dev:llm` | LLM-powered analysis with Anthropic Claude |
| `npm run dev:compare` | Broad search (2000-2025, 30 results) |
| `npm run dev:recent` | Recent papers only (2020+) |
| `npm run dev:llm-compare` | LLM mode + comparison |
| `npm run build` | Build TypeScript to dist/ |

## What You'll See

The system will:
1. **Normalize** the disease query
2. **Search** ArXiv for relevant papers
3. **Assess quality** using heuristic scoring
4. **Analyze demographics** (heuristic or LLM-powered)
5. **Identify blind spots** in research coverage
6. **Generate report** with recommendations

## Example Output

```
=== Medical Research Blind Spot Analyzer - Demo ===

📊 Analysis Mode: LLM-Powered
   ⚡ Using Anthropic Claude for demographic analysis

[Graph] Using LLM-powered population analysis
[InputAgent] Normalized: "alzheimer" -> "Alzheimer's disease"
[LiteratureSearchAgent] Found 15 papers from ArXiv
[QualityAssessmentAgent] 12 papers are high quality
[LLM-PopulationAnalysisAgent] Calling Anthropic Claude...
[LLM] Tokens used: 3,456

============================================================
EXECUTIVE SUMMARY
============================================================
Analysis identified 3 critical blind spots: pediatric exclusion,
pregnant population exclusion, and very elderly underrepresentation.
============================================================

💡 Recommendations:
   1. Include pediatric populations where ethically appropriate
   2. Include pregnant women in safety studies
   3. Increase recruitment of participants over 75
```

## Configuration Options

See [CONFIGURATION.md](CONFIGURATION.md) for detailed configuration options including:
- ArXiv API settings
- LLM model selection
- Filter customization
- Demo modes

## Troubleshooting

**Error: "Cannot find module '@langchain/anthropic'"**
- Run `npm install` to install dependencies

**Warning: "ANTHROPIC_API_KEY is not set"**
- This is expected if you haven't set up Anthropic yet
- The system will use mock LLM responses (for testing)
- To use real Anthropic API, set the `ANTHROPIC_API_KEY` environment variable

**ArXiv rate limiting**
- If you see HTTP 429 errors, wait a few minutes
- ArXiv has rate limits for API requests

## Next Steps

- Review [CONFIGURATION.md](CONFIGURATION.md) for advanced configuration
- Check [README.md](README.md) for system architecture details
- Explore different demo modes to compare heuristic vs LLM analysis
