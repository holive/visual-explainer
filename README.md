<p>
  <img src="banner.png" alt="visual-explainer" width="1100">
</p>

# visual-explainer

**An agent skill that turns any input into a beautiful, self-contained HTML visualization.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

Feed it structured data, architecture descriptions, comparison matrices, flow definitions, or free-form technical content. It picks the best visualization approach and produces a single `.html` file with real typography, dark/light theme support, and interactive Mermaid diagrams with zoom and pan. No build step, no dependencies beyond a browser.

https://github.com/user-attachments/assets/55ebc81b-8732-40f6-a4b1-7c3781aa96ec

## Why

Every coding agent defaults to ASCII art when you ask for a diagram. Box-drawing characters, monospace alignment hacks, text arrows. It works for trivial cases, but anything beyond a 3-box flowchart turns into an unreadable mess that nobody would put in a presentation or share with a team.

Tables are worse. Ask the agent to compare 15 requirements against a plan and you get a wall of pipes and dashes that wraps and breaks in the terminal. The data is there but it's painful to read.

## Install

The skill follows the [Agent Skills specification](https://agentskills.io/specification). Clone it into your agent's skills directory:

```bash
# Pi
git clone https://github.com/nicobailon/visual-explainer.git ~/.pi/agent/skills/visual-explainer

# Claude Code
git clone https://github.com/nicobailon/visual-explainer.git ~/.claude/skills/visual-explainer

# Other agents — point at the directory containing SKILL.md,
# or paste its contents into your system prompt
```

If you have [surf-cli](https://github.com/nicobailon/surf-cli) installed, the skill can also generate illustrations via Gemini and embed them in pages. The agent detects surf automatically and skips image generation if it's not there.

## Usage

Give the skill any input and it will determine the best way to visualize it. Other skills or agents handle data gathering and feed their output here for rendering. Output goes to `~/.agent/diagrams/`.

```
> visualize our authentication flow
> visualize this diff review data: { ... }
> turn this comparison into a diagram
```

The skill handles 11 diagram types automatically: architecture overviews, flowcharts, sequence diagrams, data flows, ER/schema diagrams, state machines, mind maps, data tables, timelines, dashboards, and any combination.

## How It Works

```
Input (any structured or unstructured content)
    |
SKILL.md (workflow + design principles)
    |
references/           <- agent reads before each generation
+-- css-patterns.md   (layouts, animations, theming, depth tiers)
+-- libraries.md      (Mermaid theming, Chart.js, anime.js, font pairings)
+-- responsive-nav.md (sticky sidebar TOC for multi-section pages)
    |
templates/            <- agent reads the matching reference template
+-- architecture.html (CSS Grid cards -- terracotta/sage palette)
+-- mermaid-flowchart.html (Mermaid + ELK + handDrawn -- teal/cyan palette)
+-- data-table.html   (tables with KPIs and badges -- rose/cranberry palette)
    |
~/.agent/diagrams/filename.html -> opens in browser
```

The agent analyzes the input, picks an aesthetic direction, reads the right reference template, generates a self-contained HTML file with both light and dark themes, and opens it. The three templates use deliberately different palettes so the agent learns variety rather than defaulting to one look.

To customize the output directory, browser command, or add your own diagram types and CSS patterns, edit the files directly. The agent reads them fresh each time.

## Limitations

- Requires a browser to view -- no inline terminal rendering
- Switching OS theme requires a page refresh for Mermaid SVGs (CSS-styled elements respond instantly)
- Results vary by model capability -- the skill provides design guidance, not pixel-perfect specs

## Credits

Borrows ideas from [Anthropic's frontend-design skill](https://github.com/anthropics/skills) and [interface-design](https://github.com/Dammyjay93/interface-design), adapted for one-shot diagram generation.

## License

MIT
