# AI Sentiment Analyzer

A single-page web app that classifies customer reviews as **positive**, **neutral**, or **negative**, scores each one from −1 to +1, and summarizes the overall sentiment — all client-side, no backend or API key required.

**Live demo:** https://claude.ai/artifact/DDRoJTwHLMbvnqpD9sSJnr

## Features (Feature Set C)

| Feature | Description |
|---|---|
| Multiple review inputs | Paste reviews (one per line), upload a `.txt`/`.csv` file, or load built-in samples |
| Sentiment classification | Each review labeled Positive / Neutral / Negative |
| Sentiment score | Each review scored from −1 (very negative) to +1 (very positive) |
| Overall sentiment summary | Average score, split percentages, and auto-written summary sentence |
| Display results | Spectrum plot, distribution bar, filterable review cards with highlighted keywords |

## Screenshots

**Input panel** — paste, upload, or load sample reviews:

![Input panel](screenshots/input.png)

**Summary view** — overall score, sentiment spectrum, distribution, and top praise/complaint themes:

![Summary view](screenshots/summary.png)

**Review list** — each review classified, scored, and keyword-highlighted, with filters:

![Review list](screenshots/reviews.png)

## How it works

The analyzer runs entirely in the browser using a hand-built lexicon/rule-based scoring model (no external AI API):

1. **Tokenize** the review text.
2. **Score** each word against a ~150-term weighted lexicon (e.g. `love: +3`, `terrible: -3`).
3. **Adjust** for:
   - **Negation** — "not good" flips and dampens the score.
   - **Intensity** — "very", "extremely", "slightly" scale the weight.
   - **Contrast** — words after "but" are weighted more heavily than words before it.
   - **Exclamation marks and emoji** — add a small boost.
4. **Normalize** the total to a −1…+1 score and bucket it into a label (≥ 0.15 positive, ≤ −0.15 negative, else neutral).
5. **Aggregate** all reviews into an average score, a positive/neutral/negative split, and the top praise/complaint keywords by total weight.

Because it's a transparent rule-based model rather than a trained classifier, it's fast, free, works offline, and is easy to extend — but it won't catch sarcasm or domain-specific slang the way a real NLP model would.

## Running it

No build step or server needed.

```bash
# just open it
open sentiment-analyzer.html      # macOS
start sentiment-analyzer.html     # Windows
xdg-open sentiment-analyzer.html  # Linux
```

Or double-click the file in a file browser — it's a single self-contained HTML file.

## Project structure

```
sentiment-analyzer.html   # everything: markup, CSS, and JS in one file
screenshots/              # README images
README.md
```

## Customizing

- **Lexicon:** edit the `LEX` object at the top of the `<script>` block to add/remove words or change weights.
- **Thresholds:** change the `0.15` cutoffs in the `label()` function to make classification stricter or looser.
- **Styling:** colors and fonts are defined as CSS custom properties at the top of the `<style>` block (`--pos`, `--neg`, `--neu`, etc.), including a dark-mode variant.

## Possible extensions

- Swap the rule-based scorer for a real NLP/LLM call (e.g. the Anthropic API) for higher accuracy on sarcasm and nuance.
- Add CSV export of results.
- Add per-aspect sentiment (e.g. separate scores for "price," "service," "quality").
- Persist review history with local storage.
