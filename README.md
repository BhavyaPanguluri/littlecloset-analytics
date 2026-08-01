# Little Closet — User Behavior Analysis with Markov Chains

An end-to-end project that instruments an e-commerce website with analytics, then applies a
**Markov chain model** to real clickstream data to understand user navigation patterns and
recommend UX improvements.

## Overview

Most product/UX decisions on e-commerce sites are guesswork without data. This project builds
a small e-commerce site, tracks real user sessions with Google Analytics, models the
page-to-page transitions as a Markov chain, and turns the resulting transition probabilities
into concrete UX recommendations.

**Pipeline:** Website → Instrumentation (Google Analytics + WalkMe) → Data Collection →
Data Preparation → Markov Chain Modeling → Insight Generation

## What's in this repo

| Folder | Contents |
|---|---|
| [`website/`](./website) | The e-commerce site ("Little Closet") — HTML/CSS/JS, built from a template and customized, instrumented with Google Analytics and WalkMe |
| [`analysis/`](./analysis) | `markov_chain_analysis.ipynb` — the Markov chain analysis notebook, plus the prepared clickstream dataset (`clickstream_sessions.csv`) |
| [`docs/`](./docs) | `project_report.pdf` — full write-up: methodology, heatmaps, transition diagrams, and recommendations |

## Method

1. **Site & tracking** — Deployed the site on Netlify, added a Google Analytics snippet across
   all pages, and used WalkMe to guide first-time users.
2. **Data collection** — Pulled per-user session data (JSON) from GA's User Explorer, merged
   into a single CSV, and encoded each page as a state:
   - `A1` = Home, `A2` = Categories, `A3` = Product detail, `A4` = Cart
3. **Data prep** — Converted each session into an ordered sequence of visited states
   (a "clickstream").
4. **Modeling** — Used the [`markovclick`](https://pypi.org/project/markovclick/) library to
   build a first-order Markov chain from the clickstreams, producing a transition probability
   matrix (visualized as a heatmap) and a transition graph.
5. **Insights**
   - Users overwhelmingly return to the homepage (`A1`) after visiting categories, products,
     or cart — rather than continuing to browse — likely because top products were already
     surfaced on the homepage, removing the incentive to explore further.
   - Very low probability of moving from the cart (`A4`) to categories/products — users treat
     cart as a near-terminal state.
   - Bounce rate was high overall, and acquisition via social/organic/referral channels was
     low relative to direct traffic.
   - **Recommendation:** curate (rather than over-feature) homepage products to nudge
     exploration, add "related/other products" prompts on the product page, and invest in
     organic/social acquisition.

## Tech stack

- **Frontend:** HTML, CSS, JavaScript, Bootstrap
- **Analytics:** Google Analytics (Universal Analytics — GA has since moved to GA4), WalkMe
- **Hosting:** Netlify
- **Data analysis:** Python, pandas, NumPy, seaborn/matplotlib, `markovclick`

## Running the analysis

```bash
cd analysis
pip install markovclick pandas numpy matplotlib seaborn
jupyter notebook markov_chain_analysis.ipynb
```

## Full report

See [`docs/project_report.pdf`](./docs/project_report.pdf) for the complete write-up with screenshots
of the Google Analytics dashboard, transition heatmap, and Markov chain graph.

---
*Built by [Bhavya Panguluri](https://github.com/BhavyaPanguluri) in 2019, as one of college projects in final year.*
