# The Meaning I am to Bear
A short play on words, this project is designed to extract, curatem and annotate the multiple meanings of the word "bear" given the NLTK dataset. This project implements a hybrid approach to decide if LLMs, statistical approaches, and rule based translations perform well against open-source data. I have attached the full dataset here for review.

# Bear Semantic Corpus & WSD Pipeline

A modular computational linguistics project designed to extract, curate, annotate, and evaluate the polysemous and homonymous usages of the lemma **"bear"** across classic English literature (NLTK Gutenberg corpus). This project implements a hybrid approach combining local Large Language Models (LLMs), WordNet Word Sense Disambiguation (Lesk), Bag-of-Words window heuristics, and custom syntactic decision lists.

---

## Project Architecture & Pipeline

The project is structured as a progressive multi-cell pipeline:

1. **Corpus Extraction & Synchronization (`Cell 1`)**: Scans the NLTK Gutenberg corpus for sentences containing the lemma `"bear"`, tokenizes them, applies POS tagging, and synchronizes the results with the local JSON dataset (`bear_bait.json`) using robust backup and formatting guards.
2. **LLM Annotation (`Cell 1`)**: Connects to a local LM Studio server running **Gemma 12B** to strictly classify each token into one of 9 fine-grained semantic ontology categories. *Note: You can attach the pre-packaged `bear_bait.json` dataset directly to your workspace to bypass running the local LLM API server query entirely.*
3. **Manual Overrides (`Cell 2`)**: Provides a structured dictionary (`missing_clues`) to hand-patch edge cases and disambiguate instances where automated inference fails.
4. **WordNet Lesk Evaluation (`Cell 3`)**: Implements WordNet's Lesk algorithm for baseline Word Sense Disambiguation (WSD), mapping surface forms and sentence contexts to granular synset definitions.
5. **Bag-of-Words Heuristics (`Cell 5`)**: Evaluates a point-system bag-of-words classifier using balanced 100-word keyword windows for each semantic category.
6. **Syntactic Rule Engine (`Cell 6`)**: Implements a rigorous deterministic syntactic decision-list parser (`classify_bear_syntax`) targeting auxiliary chains, compound nouns, modal contexts, idiom windows, and clause structures.

---

## Semantic Ontology Categories

The dataset classifies target instances into 9 precise part-of-speech-tagged categories:

### Nouns (`NOUN_BEARS`)
* `n_ANIMAL`: Biological mammal of the family Ursidae (*e.g., "The brown bear hunted salmon"*).
* `n_AGENT`: Person, entity, or mechanical part carrying a burden or message (*e.g., "pall-bearer"*).
* `n_GEOGRAPHY`: Spatial orientation, compass headings, or navigational bearings (*e.g., "get my bearings"*).
* `n_COMPOUND`: Inseparable part of a proper noun or place name (*e.g., "Bear Mountain"*).

### Verbs (`VERB_BEARS`)
* `v_CARRY_PHYSICALLY`: Holding, supporting, or transporting a physical load (*e.g., "bear a heavy box"*).
* `v_ENDURE`: Tolerating hardship, pain, or adversity (*e.g., "cannot bear the pain"*).
* `v_GIVE_BIRTH`: Biological reproduction or agricultural yield (*e.g., "apple tree will bear fruit"*).
* `v_SYMBOLICALLY`: Possessing an abstract trait, title, or resemblance (*e.g., "bears the title of King"*).
* `v_IDIOM`: Fixed idiomatic expressions and phrasal verbs (*e.g., "bear in mind", "bear with me"*).

---

## Requirements & Installation

Ensure you have Python 3.10+ installed along with the required natural language processing libraries:

```bash
pip install nltk openai pathlib
