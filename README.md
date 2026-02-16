# Quranic Arabic Curriculum

Open-source, structured curriculum data for learning Quranic Arabic — organized by linguistic concept, grounded in the [Quranic Arabic Corpus](https://corpus.quran.com) (University of Leeds).

Use this data to build Quranic Arabic learning apps, research tools, or educational resources in any language or platform.

## Data Files

### `data/curriculum.json`

The full learning path: 2 sections, 16 units, 61 steps, 126 vocabulary words.

```
Section → Unit → Step → (3 lessons generated at runtime: intro, practice, master)
```

**Sections:**
1. **The Essentials** — Al-Fatiha, common particles, pronouns, salah phrases, Al-Ikhlas
2. **Building Blocks** — Allah's Names, seeking protection, good & evil, time & loss, belief & action, creation, negation

**Each unit contains:**
- `vocabulary` — Arabic words with translation, transliteration, Quran frequency, and corpus reference
- `steps` — Learning progression with vocab indices mapping to the unit's vocabulary
- `anchorAyahs` — Quran verses that anchor the unit's concept
- `grammarTip` — Optional grammar explanation with examples
- `rootFamilies` — Arabic root letters linking to the roots file

### `data/roots.json`

62 Quranic Arabic root families with derived words.

Each root includes:
- `rootLetters` — Arabic root (e.g. `ر ح م`)
- `coreMeaning` — Core semantic meaning (e.g. "mercy, compassion")
- `sourceRef` — Buckwalter code for corpus lookup (e.g. `rHm`)
- `words` — Derived vocabulary with Arabic, translation, and transliteration

## Frequency Data

Every vocabulary word with a root has a verified `quranFrequency` — the **lemma frequency** from the QAC dictionary (count of all forms of that dictionary entry across the entire Quran).

To verify any frequency:
1. Take the word's `frequencyRef` (format: `root:lemma` in Buckwalter transliteration)
2. Go to `corpus.quran.com/qurandictionary.jsp?q={root}`
3. Find the lemma row — the count should match `quranFrequency`

Words without roots (particles, pronouns, multi-word phrases) have `quranFrequency: 0` and no `frequencyRef`.

## Sources

| Data | Source |
|------|--------|
| Word roots & frequencies | [Quranic Arabic Corpus](https://corpus.quran.com) — University of Leeds |
| Grammar | Standard Arabic linguistics (nahw, sarf, balaagha) |
| Quran text | [Tanzil.net](https://tanzil.net) open-source Quran project |

## Schema

<details>
<summary>curriculum.json structure</summary>

```json
{
  "version": "1.0.0",
  "source": "...",
  "sections": [
    {
      "order": 1,
      "name": "The Essentials",
      "description": "...",
      "themeColor": "#ff4caf50",
      "units": [
        {
          "id": "cs1_u1",
          "name": "In the Name of Allah",
          "concept": "Definite article ال & Bismillah",
          "description": "...",
          "order": 1,
          "grammarTip": {
            "title": "...",
            "explanation": "...",
            "sourceNote": "...",
            "examples": [
              { "arabic": "...", "english": "...", "transliteration": "..." }
            ]
          },
          "rootFamilies": ["ر ح م", "..."],
          "vocabulary": [
            {
              "arabic": "بِسْمِ",
              "translation": "in the name of",
              "transliteration": "bismi",
              "rootLetters": "س م و",
              "quranFrequency": 19,
              "frequencyRef": "smw:<som",
              "exampleAyah": { "surahId": 1, "ayahId": 1 }
            }
          ],
          "anchorAyahs": [{ "surahId": 1, "ayahId": 1 }],
          "steps": [
            {
              "id": "cs1_u1_s1",
              "name": "Bismillah",
              "order": 1,
              "vocabIndices": [0, 1, 2]
            }
          ]
        }
      ]
    }
  ]
}
```
</details>

<details>
<summary>roots.json structure</summary>

```json
{
  "version": "1.0.0",
  "source": "...",
  "roots": {
    "ر ح م": {
      "rootLetters": "ر ح م",
      "coreMeaning": "mercy, compassion",
      "sourceRef": "rHm",
      "words": [
        {
          "arabic": "رَحْمَة",
          "translation": "mercy",
          "transliteration": "rahma"
        }
      ]
    }
  }
}
```
</details>

## Usage

The data is plain JSON — use it in any language:

```python
import json

with open('data/curriculum.json') as f:
    curriculum = json.load(f)

for section in curriculum['sections']:
    for unit in section['units']:
        print(f"{unit['name']} — {len(unit['vocabulary'])} words")
```

```javascript
const curriculum = require('./data/curriculum.json');

curriculum.sections.flatMap(s => s.units).forEach(unit => {
  console.log(`${unit.name} — ${unit.vocabulary.length} words`);
});
```

## Contributing

Contributions welcome! If you find a frequency error or want to improve the curriculum:

1. Fork this repo
2. Edit the JSON files directly
3. Include your source (e.g. corpus.quran.com link) in the PR description
4. Submit a pull request

All frequency claims should be verifiable against the [Quranic Arabic Corpus dictionary](https://corpus.quran.com/qurandictionary.jsp).

## License

[CC BY-SA 4.0](LICENSE) — free to use, share, and adapt with attribution.
