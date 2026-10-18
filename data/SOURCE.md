# Source document

| | |
|---|---|
| Document | *Laws of the Game 2024/25* |
| Publisher | The International Football Association Board (IFAB) |
| File | `Laws-of-the-Game-2024_25.pdf` |
| Pages | 116 |
| Where from | IFAB download server: https://downloads.theifab.com/downloads/laws-of-the-game-2024-25?l=en (English, effective 1 July 2024; the PDF's own cover page points to www.theifab.com). The IFAB has since published a newer edition, so this file is the 2024/25 one |
| Committed to the repo? | No. The PDF is listed in `.gitignore` because it is not ours to redistribute. Download it from the publisher and place it in this folder |

## Notes for chunking

- Text is extractable (not a scan). Plain `pdftotext` or `pypdf` extraction works.
- Roughly half of the extracted lines are short fragments from diagrams, captions and headings.
- Leave out the front matter, the glossary and the "what changed" summary. They duplicate real rules and
  would add near-identical chunks.
