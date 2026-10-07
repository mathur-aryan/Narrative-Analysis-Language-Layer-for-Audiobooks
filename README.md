## "Turns any book into a fully dramatized audiobook where every character has their own unique voice"
[narrativecast_technical_proposal.md](https://github.com/mathur-aryan/Narrative-Analysis-Language-Layer-for-Audiobooks/blob/main/README.md)

# NarrativeCast: Autonomous Multi-Voice Audio Drama Synthesis Engine
---

## 1. Project Name
**NarrativeCast: Autonomous Multi-Voice Audio Drama & Character Dialogue Synthesis Engine**

---

## 2. Problem Statement
The global audiobook industry relies almost exclusively on single-narrator or dual-narrator vocalization paradigms. While commercial audio dramatizations (e.g., GraphicAudio) provide immersive multi-actor performances, they require prohibitive studio budgets, human casting directors, and manual post-production audio engineering—restricting full-cast treatment to a tiny fraction of bestselling commercial literature. 

Light novels, indie web fiction, serialized web novels, fan translations, and niche literature represent millions of narrative works that will never receive studio adaptations. However, converting these written works into cast-grade audio presents three fundamental technical bottlenecks:
1. **Unattributed & Elliptical Dialogue Attribution:** In narrative literature, over $40\%$ of direct speech lines lack explicit speech tags (e.g., *"he said"*, *"whispered Alice"*). Pronoun resolution, multi-party conversation tracking, and unspoken internal monologues require deep narrative discourse analysis that traditional rule-based parsers fail to resolve.
2. **Contextual Emotional Incongruence:** Standard single-speaker Text-to-Speech (TTS) engines read text flatly or apply a uniform prosody, ignoring micro-contextual shifts (e.g., sarcasm, panic, whispering under duress, or gradual emotional breakdowns).
3. **Absence of Human-in-the-Loop (HITL) Verification:** Existing automated TTS tools attempt blind end-to-end rendering without intermediate inspection layers. When an attribution error occurs, downstream acoustic generation hallucinates mismatched character timbres across thousands of tokens with zero granular intervention capabilities.

---

## 3. Project Overview
**NarrativeCast** is an end-to-end, open-source AI publishing pipeline that ingests unannotated written literary text (EPUB, PDF, TXT, Markdown) and automatically parses, extracts, attributes, casts, and renders it into a fully orchestrated, multi-voice audio drama. 

Operating on a modular, agentic multi-stage architecture, NarrativeCast decomposes prose into an intermediate structured screenplay artifact—the **Canonical Drama Script (CDS)**. In this intermediate representation, every line is classified by utterance type (direct dialogue, internal thought, or third-person exposition), attributed to a distinct entity ID, and scored with explicit affective acoustic markers (pitch variance, rate, pacing, and emotional vectors). 

Before audio synthesis, NarrativeCast presents an interactive script verification interface where creators can audit speaker attributions, adjust voice mappings, and preview emotional intonation. Once validated, the engine coordinates open-source neural TTS models and zero-shot voice cloning frameworks to generate speech audio, stitching together distinct character acoustic profiles into a unified master audio file.

---

## 4. Proposed Solution
NarrativeCast solves the automated dramatization problem through a three-tier decoupled pipeline:

```
[Raw Literary Text (EPUB/TXT)]
              │
              ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Stage 1: Linguistic Parsing & Discourse Attribution    │
 │ (Open-Weight LLMs + Discourse Coreference Graph)       │
 └─────────────────────────────────────────────────────────┘
              │
              ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Stage 2: Canonical Drama Script (CDS) Intermediate Rep  │
 │ (Speaker Attribution, Affect Scoring, Human Audit Loop) │
 └─────────────────────────────────────────────────────────┘
              │
              ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Stage 3: Multi-Speaker Acoustic Synthesis Engine        │
 │ (Kokoro-82M / Chatterbox TTS + Dynamic Timbre Staging)   │
 └─────────────────────────────────────────────────────────┘
              │
              ▼
[Master Multi-Voice Audio Drama (FLAC / M4B / MP3 + Timed Cue Sheet)]
```

1. **Context-Aware Speaker Attribution & Tag Stripping:** An open-weight LLM agent pipeline analyzes sliding narrative windows ($2,048$ to $4,096$ tokens) with bidirectional context to reconstruct conversational turn structures, resolve floating pronouns, and prune inlined narrative speech tags (e.g., transforming `“Stop right there,” Alice snarled.` into speaker: `Alice`, tone: `aggressive/snarl`, spoken content: `“Stop right there.”`).
2. **Dynamic Affect & Modality Tagging:** Each segment is annotated with acoustic performance metadata:
   - `dialogue`: External vocalization with conversational prosody.
   - `thought`: Internal monologue rendered with subtle spatial reverb, low-pass equalization, and subdued intimate dynamics.
   - `narration`: Neutral, authoritative cadence delivered by an omniscient storyteller persona.
3. **Human-in-the-Loop (HITL) Intermediate Review:** Authors and listeners can inspect, verify, and modify the synthesized Canonical Drama Script via an intuitive visual screenplay editor, preventing catastrophic casting errors prior to GPU-intensive acoustic inference.
4. **Autonomous Voice Casting & Neural Synthesis:** Character entity profiles automatically map to distinctive timbre profiles from an open-source voice bank (leveraging Kokoro-82M and Chatterbox TTS), matching gender, age, cadence, and temperamental parameters, culminating in automated multi-track audio mastering.

---

## 5. Objectives
- **Attribution Accuracy:** Achieve $>92\%$ speaker identification accuracy across complex, multi-party literary dialogues without explicit speaker tags.
- **Structural Tag Disentanglement:** Reliably strip reporting clauses (*"she cried out frantically"*) while converting adverbial descriptors into continuous prosody control tokens for TTS synthesis.
- **Compute Efficiency on Commodity Hardware:** Enable full-scale local inference (script attribution and speech generation) on consumer-grade developer hardware (single 8GB–16GB VRAM GPU or Apple Silicon unified memory).
- **Deterministic Script Intermediate Representation:** Formulate an extensible, schema-validated JSON/YAML specification (Canonical Drama Script) that decouples linguistic narrative comprehension from acoustic rendering.
- **Accessible Open-Source Tooling:** Deliver a modular, CLI and Web-accessible workbench that requires zero proprietary API keys or paid cloud subscriptions.

---

## 6. Target Users / Use Cases
| User Segment | Primary Use Case | Value Proposition |
| :--- | :--- | :--- |
| **Independent & Web Fiction Authors** | Indie writers publishing serial web novels (Royal Road, Wattpad, Substack). | Produce studio-style multi-voice audio adaptations of their catalogs at near-zero marginal production cost. |
| **Anime & Light Novel Enthusiasts** | Readers consuming unadapted or fan-translated light novels. | Experience immersive, anime-style drama CD audiobooks with distinct, expressive character acting. |
| **Visually Impaired & Neurodivergent Readers** | Accessibility consumers requiring screen reading and audio immersion. | Eliminates narrative exhaustion from flat, robotic single-voice screen readers by providing clear, distinct audio cues for conversational turns. |
| **Audiobook Narrators & Small Studios** | Professional post-production teams. | Automates initial line attribution, character breakdown logging, and temp-track drafting, cutting pre-production studio prep time by $70\%$. |

---

## 7. Open-Source AI Technology Selected
NarrativeCast is built exclusively on top of state-of-the-art open-source and open-weight AI architectures:

1. **Primary Reasoning & Attribution Engine:**
   - **Google Gemma 4 Architecture (9B-Instruct & 2B-Instruct quantized via GGUF / AWQ):** Employed for multi-turn conversational discourse tracking, latent speaker identification, tag stripping, and emotional tone extraction.
   - **Mistral 7B Instruct (v0.3) / Qwen 2.5 7B (as alternative interchangeable local backends):** For benchmarking low-resource attribution performance.
2. **Natural Language Processing & Coreference Harness:**
   - **spaCy (v3.7+) & fastcoref:** Tokenization, dependency parsing, sentence boundary disambiguation (SBD), and antecedent entity cluster extraction.
3. **Neural Speech Synthesis & Acoustic Generation:**
   - **Kokoro-82M (Apache 2.0 Licensed):** Ultra-fast, lightweight neural TTS architecture generating studio-quality $24\text{ kHz}$ natural audio at $15\times$ real-time on consumer hardware.
   - **Chatterbox TTS / Bark / StyleTTS2 (Open-Weight Implementations):** Expressive zero-shot acoustic rendering, enabling whisper dynamics, internal thought reverb processing, and vocal cloning from short reference samples.
4. **Local Model Inference Acceleration:**
   - **vLLM / llama.cpp (via `llama-cpp-python`):** High-throughput, quantized local tensor execution utilizing continuous batching and PagedAttention.

---

## 8. Why This Technology Was Selected
```
┌────────────────────────────────────────────────────────────────────────┐
│                        ENGINEERING SELECTION CRITERIA                  │
├──────────────────────┬───────────────────────┬─────────────────────────┤
│ Requirement          │ Open-Source Choice    │ Selection Rationale     │
├──────────────────────┼───────────────────────┼─────────────────────────┤
│ Discourse Tracking   │ Gemma / Qwen /        │ Superior reasoning in   │
│ & Deep Attribution   │ llama.cpp             │ multi-turn contexts;    │
│                      │                       │ zero API costs; local   │
│                      │                       │ data privacy.           │
├──────────────────────┼───────────────────────┼─────────────────────────┤
│ Coreference Bounds   │ fastcoref / spaCy     │ Fast heuristic pruning; │
│                      │                       │ anchors entity tokens   │
│                      │                       │ prior to LLM reasoning.│
├──────────────────────┼───────────────────────┼─────────────────────────┤
│ High-Speed Acoustic  │ Kokoro-82M            │ Exceptional MOS (4.4+); │
│ Generation           │                       │ footprint < 350MB;      │
│                      │                       │ 15x real-time speed.    │
├──────────────────────┼───────────────────────┼─────────────────────────┤
│ Audio Engineering    │ FFmpeg + PyDub        │ Lossless stitching;     │
│ Pipeline             │                       │ precise multi-channel   │
│                      │                       │ filter graphs.          │
└──────────────────────┴───────────────────────┴─────────────────────────┘
```
- **Local Sovereignty & Uncapped Token Throughput:** Processing a standard 100,000-word book requires processing over 140,000 tokens through linguistic analyzers. Commercial closed APIs (e.g., OpenAI, Anthropic, ElevenLabs) impose rate limits, high costs (\$40–\$150 per book), and potential censorship of creative writing. Open-weight inference runs locally at zero marginal cost.
- **Architectural Specialization:** Kokoro-82M achieves audio quality comparable to multi-billion parameter commercial engines while requiring under 1GB of memory. It runs comfortably alongside an 8-bit or 4-bit quantized Gemma LLM on a consumer workstation.
- **Determinism and Structured Grammar Enforcement:** Using `llama-cpp-python` and vLLM enables constrained JSON schema decoding (GBNF grammars / Outlines), guaranteeing that LLM responses strictly validate against the Canonical Drama Script schema without formatting errors.

---

## 9. AI's Role in the System
AI operates in three distinct, tightly controlled stages rather than an opaque, end-to-end black box:

1. **Latent Narrative State Modeling (Discourse Tracking):**
   The linguistic model maintains a rolling mental state of active participants in a scene. When encountering a string of bare dialogue with no tags, the model evaluates conversational cadence, physical placement of characters in preceding paragraphs, idiosyncratic speech patterns, and emotional stakes to compute the probability distribution:
   $$P(\text{Speaker} = C_i \mid \text{Dialogue}_k, \text{Context}_{k-N \dots k+M})$$
2. **Prosodic Transduction & Affect Scoring:**
   The AI reads narrative adverbs and contextual clues, transducing prose nuances into concrete acoustic parameters:
   - *"she muttered, trembling"* $\longrightarrow$ `{"pitch_shift": -0.15, "speaking_rate": 0.88, "breathiness": 0.8, "emotion": "fearful_subdued"}`
3. **Neural Acoustic Waveform Synthesis:**
   The neural TTS synthesizers transform phonemized linguistic representations into high-fidelity mel-spectrograms and audio waveforms conditioned on character voice embeddings.

---

## 10. System Architecture

```
                  ┌─────────────────────────────────────────┐
                  │          Raw Document Ingestion         │
                  │        (EPUB, PDF, TXT, Markdown)       │
                  └────────────────────┬────────────────────┘
                                       │
                                       ▼
                  ┌─────────────────────────────────────────┐
                  │    Document Normalizer & Ingestion      │
                  │   - Chapter Segmentation                │
                  │   - Text Hygiene & Regex Cleaning       │
                  └────────────────────┬────────────────────┘
                                       │
                                       ▼
                  ┌─────────────────────────────────────────┐
                  │     Syntactic Boundary & Entity Graph   │
                  │   - Sentence Boundary Split (spaCy)     │
                  │   - Fastcoref Entity Extraction         │
                  └────────────────────┬────────────────────┘
                                       │
                                       ▼
       ┌───────────────────────────────────────────────────────────────┐
       │             LLM Semantic Processing (Agent Engine)            │
       │  ┌───────────────────────┐         ┌───────────────────────┐  │
       │  │ Dialogue Attribution │         │ Affect & Tone Scoring │  │
       │  │   & Tag Stripping     │         │   (Emotion Analysis)  │  │
       │  └───────────┬───────────┘         └───────────┬───────────┘  │
       │              └───────────────┬─────────────────┘              │
       │                              ▼                                │
       │                Constrained Schema Validator                   │
       │                     (Pydantic / GBNF)                         │
       └──────────────────────────────┬────────────────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │   Canonical Drama Script (CDS) Engine   │
                  │         - Standardized JSON/YAML        │
                  └───────────────────┬─────────────────────┘
                                      │
                     ┌────────────────┴────────────────┐
                     │                                 │
                     ▼                                 ▼
       ┌───────────────────────────┐     ┌───────────────────────────┐
       │   Developer CLI Mode      │     │  Interactive Web Studio   │
       │  - Headless batch run     │     │  - Attribution editor     │
       │  - Automated processing   │     │  - Voice audition & cast  │
       └─────────────┬─────────────┘     └─────────────┬─────────────┘
                     │                                 │
                     └────────────────┬────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │     Acoustic Orchestration Pipeline     │
                  │   - Character Voice Mapping             │
                  │   - Kokoro-82M / Chatterbox TTS         │
                  │   - DSP Audio Master Engine (FFmpeg)    │
                  └───────────────────┬─────────────────────┘
                                      │
                                      ▼
                  ┌─────────────────────────────────────────┐
                  │           Final Master Artifacts        │
                  │    .M4B (Audiobook) / .FLAC / .VTT      │
                  └─────────────────────────────────────────┘
```

---

## 11. Component-Level Architecture

### A. Document Parsing & Text Normalizer
- Extracts clean text from EPUB/PDF, removing metadata, tables of contents, and formatting artifacts.
- Segments text into contiguous narrative scenes based on whitespace, scene breaks (`***`), and chapter demarcations.

### B. Syntactic Pre-Processor & Entity Extractor
- Employs `spaCy` to construct dependency trees and identify candidate Named Entities (`PERSON`, `ORG`).
- Initializes a **Scene Entity Registry** tracking characters currently present in the narrative space.

### C. Agentic Attribution & Acoustic Extraction Engine
- Implements a structured prompt orchestrator backed by `llama-cpp-python` / `vLLM`.
- Evaluates dialogue lines using sliding bidirectional windows ($1,024$ tokens look-behind, $512$ tokens look-ahead).
- Disentangles reported narrative clauses from spoken dialogue, returning clean utterance strings, speaker IDs, and acoustic modifiers.

### D. Canonical Drama Script (CDS) Data Store
- Stores intermediate representations in an immutable, validated JSON format.
- Contains rich metadata for every spoken turn: timestamps, speaker IDs, confidence metrics, utterance types, and audio processing directives.

### E. Human-in-the-Loop (HITL) Web Studio
- A lightweight React/Vite web interface that displays dialogue color-coded by speaker.
- Provides immediate playback previews, one-click speaker re-assignment, and fine-tuning sliders for tone, pitch, and speed.

### F. Neural Voice Synthesis Worker Pool
- Orchestrates asynchronous speech synthesis jobs across CPU/GPU worker threads.
- Matches speaker IDs to specific voice profiles (timbre embeddings, pitch offsets, speaking rates).
- Caches generated audio chunks indexed by content hash and voice parameters to avoid re-rendering identical lines.

### G. Audio Master DSP Engine
- Stitches synthesized audio chunks using `PyDub` and `FFmpeg`.
- Applies character-specific audio filters (e.g., subtle room reverb and high-shelf dampening for internal monologues, clean stereo presence for dialogue).
- Inserts dynamic pause durations based on scene transitions, punctuation marks, and dramatic tension markers.
- Compiles the final master file into `.m4b` format complete with chapter markers and synchronous `.vtt`/`.lrc` subtitle cue sheets.

---

## 12. Data / Information Flow

```
[Raw Book Document: EPUB/TXT]
               │
               ▼
[Chapter Text Splitting & Cleaning]
               │
               ▼
[Pre-Entity Extraction & Token Chunking (spaCy)]
               │
               ▼
[Structured LLM Prompting with Constrained JSON Grammar]
               │
               ▼
┌─────────────────────────────────────────────────────────────┐
│ Canonical Drama Script (CDS) Record:                        │
│ {                                                           │
│   "scene_id": "04_confrontation",                           │
│   "turns": [                                                │
│     {                                                       │
│       "turn_id": 102,                                       │
│       "type": "narration",                                  │
│       "speaker": "NARRATOR",                                │
│       "text": "The wind howled through the cracked window.",│
│       "delivery": {"pacing": 0.95, "reverb": "dry"}         │
│     },                                                      │
│     {                                                       │
│       "turn_id": 103,                                       │
│       "type": "dialogue",                                   │
│       "speaker": "Alice",                                   │
│       "text": "Put the grimoire down!",                     │
│       "delivery": {                                         │
│         "emotion": "urgent_anger",                          │
│         "pitch_shift": 1.1,                                 │
│         "speed": 1.15                                       │
│       }                                                     │
│     },                                                      │
│     {                                                       │
│       "turn_id": 104,                                       │
│       "type": "thought",                                    │
│       "speaker": "Bob",                                     │
│       "text": "She has no idea what it's capable of.",      │
│       "delivery": {                                         │
│         "emotion": "somber_internal",                       │
│         "dsp_filter": "whisper_reverb"                      │
│       }                                                     │
│     }                                                       │
│   ]                                                         │
│ }                                                           │
└──────────────────────────────┬──────────────────────────────┘
                               │
            ┌──────────────────┴──────────────────┐
            │ [Human Audit: Confirm / Reassign]   │
            └──────────────────┬──────────────────┘
                               │
                               ▼
[Voice Profile Binding: Alice -> Voice_A, Bob -> Voice_B, Narrator -> Voice_C]
               │
               ▼
[Multi-Threaded Audio Generation: Kokoro-82M / Chatterbox TTS]
               │
               ▼
[DSP Concatenation, Reverb Filtering, Pause Timing (FFmpeg)]
               │
               ▼
[Master M4B Audiobook with Chapters + Synchronized Subtitle Cues]
```

---

## 13. Agentic Workflow
NarrativeCast employs a multi-step agentic workflow designed for literary discourse analysis:

```
                  ┌──────────────────────────────┐
                  │       Ingested Scene         │
                  └──────────────┬───────────────┘
                                 │
                                 ▼
   ┌─────────────────────────────────────────────────────────────┐
   │ Agent 1: Scene & Entity Profiler                            │
   │ - Identifies all present characters in the scene window.    │
   │ - Extracts physical descriptors and emotional states.       │
   └─────────────────────────────┬───────────────────────────────┘
                                 │
                                 ▼
   ┌─────────────────────────────────────────────────────────────┐
   │ Agent 2: Discourse Turn Extractor                           │
   │ - Scans text for quotes, thoughts, and narration blocks.    │
   │ - Strips attribution clauses ("she whispered softly").      │
   │ - Transduces descriptive adverbs into prosody parameters.   │
   └─────────────────────────────┬───────────────────────────────┘
                                 │
                                 ▼
   ┌─────────────────────────────────────────────────────────────┐
   │ Agent 3: Attribution & Coreference Reasoner                 │
   │ - Resolves unattributed utterances against active entities. │
   │ - Tracks alternating conversational turns (A -> B -> A).    │
   │ - Assigns confidence score (0.0 to 1.0) to every turn.     │
   └─────────────────────────────┬───────────────────────────────┘
                                 │
                                 ▼
                 ┌───────────────────────────────┐
                 │ Confidence Check >= 0.85?     │
                 └───────┬───────────────┬───────┘
                         │               │
                   [YES] │               │ [NO]
                         ▼               ▼
         ┌──────────────────┐    ┌───────────────────────────────────┐
         │ Commit to CDS    │    │ Agent 4: Disambiguation Critic    │
         │ intermediate     │    │ - Expands context window backwards│
         │ script           │    │ - Re-evaluates linguistic markers │
         └──────────────────┘    │ - Flags low-confidence lines for  │
                                 │   human audit in Web Studio       │
                                 └───────────────────────────────────┘
```

1. **Scene & Entity Profiler:** Scans the incoming narrative scene to establish the active conversational cast, pruning inactive characters to keep attribution accurate.
2. **Discourse Turn Extractor:** Isolates spoken dialogue, internal thought blocks, and background narration, converting prose descriptors into structured prosody tags.
3. **Attribution & Coreference Reasoner:** Resolves speaker identities using bidirectional context and alternating turn heuristics, assigning an explicit confidence score to each classification.
4. **Disambiguation Critic:** Automatically triggers when attribution confidence falls below a preset threshold ($85\%$). It expands the contextual window, re-evaluates preceding dialogue exchanges, and flags low-confidence turns for priority human review.

---

## 14. Technology Stack
- **Core Programming Language:** Python 3.10+ (Type-annotated, asynchronous pipeline architecture).
- **LLM Reasoning & Inference Framework:**
  - `llama-cpp-python` / `vLLM` for fast, local quantized model inference.
  - Open-weight models: Gemma 2/4 (2B/9B Instruct), Qwen 2.5 7B Instruct.
  - Structured output parsing: Pydantic v2 + Outlines / GBNF Grammar.
- **NLP & Parsing Tools:**
  - `spaCy` v3.7 (fast rule-based sentence segmentation & entity recognition).
  - `fastcoref` (Python coreference resolution engine).
  - `ebooklib` & `BeautifulSoup4` (lossless EPUB and HTML parsing).
- **Audio Synthesis & Acoustic Engines:**
  - `Kokoro-82M` (ONNX / PyTorch execution runtime).
  - `Chatterbox TTS` / `StyleTTS2` (dynamic timbre voice cloning).
  - `soundfile` & `numpy` (high-performance audio tensor manipulation).
- **Audio Processing & Mastering:**
  - `FFmpeg` (audio normalization, DSP filtering, dynamic range compression).
  - `PyDub` (micro-pacing, silence insertion, and track concatenation).
- **Web Interface & Frontend:**
  - React 18 + Vite + Tailwind CSS.
  - Wavesurfer.js (waveform visualization and audio scrubbing).
  - FastAPI (backend REST API connecting the web UI to the inference engine).

---

## 15. Expected Features
- **Accurate Speaker Attribution:** Resolves unattributed literary dialogue across complex, multi-party conversations using deep LLM discourse reasoning.
- **Utterance Tri-Partite Classification:** Automatically categorizes text into external dialogue, internal thoughts, and third-person narration.
- **Automated Dialogue Tag Pruning:** Strips reporting tags (*"he said"*, *"whispered the girl"*) from spoken audio while preserving narrative flow in the narration voice.
- **Dynamic Prosodic & Emotion Control:** Infers appropriate pitch, speaking rate, and emotional intonation directly from contextual cues and narrative descriptors.
- **Canonical Drama Script (CDS) Export:** Generates clean, human-readable intermediate scripts in JSON, YAML, and Fountain (screenplay) formats.
- **Interactive Script Review Studio:** A focused web interface for reviewing, editing, and confirming character attributions and voice mappings before audio generation.
- **High-Speed Local Audio Rendering:** Rapid synthesis running at up to $15\times$ real-time using Kokoro-82M on consumer GPUs.
- **Distinct Internal Monologue Processing:** Automatically applies acoustic filters (subtle reverb, intimacy EQ) to internal thoughts for clear audio separation.
- **Audiobook-Ready Master Packaging:** Compiles multi-voice audio into `.m4b` format with chapter divisions and timed `.vtt`/`.lrc` subtitles.

---

## 16. Implementation Approach

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DEVELOPMENT ROADMAP PHASES                      │
├─────────────────┬──────────────────────────────────────────────────────┤
│ Phase           │ Key Deliverables & Milestones                        │
├─────────────────┼──────────────────────────────────────────────────────┤
│ Phase 1:        │ • Universal text parser (EPUB, TXT, Markdown).       │
│ Preprocessing   │ • Scene and paragraph segmentation engine.           │
│ & Parsing       │ • Sentence boundary disambiguation with spaCy.       │
├─────────────────┼──────────────────────────────────────────────────────┤
│ Phase 2:        │ • Prompt engine with strict Pydantic JSON schema.    │
│ Attribution &   │ • Sliding-window context pipeline with Gemma.        │
│ Discourse LLM   │ • Tag-stripping and emotion classification module.   │
├─────────────────┼──────────────────────────────────────────────────────┤
│ Phase 3:        │ • Canonical Drama Script (CDS) validation engine.    │
│ Script & Review │ • Interactive Web Studio for visual line auditing.   │
│ Engine          │ • Timbre assignment and character voice palette.     │
├─────────────────┼──────────────────────────────────────────────────────┤
│ Phase 4:        │ • Multi-worker TTS queue with Kokoro-82M.            │
│ Synthesis &     │ • DSP audio master pipeline using FFmpeg / PyDub.    │
│ Audio Mastering │ • M4B chapter packaging and subtitle export.         │
└─────────────────┴──────────────────────────────────────────────────────┘
```

The system will be developed and evaluated using established literary benchmarks, including the **Project Gutenberg Character Attribution Corpus** and custom multi-character dialogue scenes from modern light novels.

---

## 17. Expected Final Output
1. **Canonical Drama Script (CDS) File:** A clean, schema-validated `.json` file containing structured dialogue turns, speaker attributions, and emotional performance parameters.
2. **Master Multi-Voice Audio Drama:** A high-fidelity, mastered audio file (`.m4b` / `.flac` / `.mp3`) featuring distinct character voices, balanced dialogue volume, and contextual audio effects.
3. **Synchronized Subtitle Cue Sheets:** Synchronous `.vtt` and `.lrc` subtitle files containing sentence-level timestamps and speaker tags for accessible reading.
4. **Interactive Web Studio:** A locally hosted, intuitive web interface allowing creators to review scripts, audition voices, adjust pacing, and trigger batch rendering.

---

## 18. Future Scope / Scalability
- **Procedural Ambient Soundscapes (BGM & SFX):** Integrating open-source audio generation models (e.g., AudioCraft, AudioLDM) to automatically generate subtle background ambience and foley effects based on scene descriptions (e.g., crackling fires, falling rain, distant thunder).
- **Multilingual Support & Anime Voice Profiles:** Fine-tuning voice models on open multilingual datasets to generate multi-speaker audio dramas in Japanese, Hindi, Spanish, and other languages.
- **Distributed Audio Generation:** Scaling the synthesis pipeline using Ray or Celery worker pools across multiple consumer machines or local cluster setups.
- **Zero-Shot Voice Cloning via Reference Audio:** Enabling creators to supply a 10-second reference audio clip to automatically generate consistent, unique character voices throughout an entire book series.

---

## 19. Open-Source Dependencies / Components
```
# Core Language & LLM Inference
python >= 3.10
llama-cpp-python >= 0.2.89      # Hardware-accelerated local GGUF inference
vllm >= 0.5.0                   # High-throughput batch inference engine
pydantic >= 2.6.0               # Data validation and schema enforcement
outlines >= 0.0.40              # Guided generation & schema enforcement

# NLP & Text Analysis
spacy >= 3.7.4                  # Sentence boundary detection and NLP
fastcoref >= 2.1.0              # Fast neural coreference resolution
ebooklib >= 0.18                # EPUB archive extraction
beautifulsoup4 >= 4.12.3        # HTML text sanitation

# Neural TTS & Audio Processing
kokoro >= 0.1.0                 # Lightweight 82M neural TTS synthesizer
torch >= 2.2.0                  # Deep learning framework
torchaudio >= 2.2.0             # Audio processing operations
soundfile >= 0.12.1             # Audio export and reading
pydub >= 0.25.1                 # Audio mastering and segment manipulation
ffmpeg-python >= 0.2.0          # FFmpeg binding for DSP mastering

# Web Application & Review Interface
fastapi >= 0.110.0              # Async REST backend
uvicorn >= 0.28.0               # High-performance ASGI server
wavesurfer.js >= 7.0.0          # Interactive audio waveform visualization
react >= 18.2.0                 # UI library
tailwindcss >= 3.4.0            # Frontend styling
```

---

## 20. Expected Challenges and Mitigation
| Challenge | Root Cause | Technical Mitigation Strategy |
| :--- | :--- | :--- |
| **Ambiguous Pronouns in Extended Dialogue** | Writers omit speaker names across long conversational exchanges (e.g., 10+ turns of continuous quotes). | Maintain an explicit Scene Entity Stack. Apply conversational alternation heuristics ($A \leftrightarrow B$) and expand the LLM's look-back window to locate earlier speaker anchors. |
| **Over-pruning or Hallucinatory Line Stripping** | The LLM may accidentally strip actual spoken dialogue when attempting to remove narrative tags. | Enforce strict substring alignment checks. The cleaned dialogue must map directly back to a contiguous substring of the original text, preventing hallucinated words. |
| **Inference Latency during Audio Generation** | Synthesizing tens of thousands of individual speech lines can create processing bottlenecks. | Use Kokoro-82M for fast inference ($15\times$ real-time). Implement parallel batch synthesis and cache rendered audio chunks by text and voice hash. |
| **Acoustic Volume & Quality Inconsistencies** | Concatenating audio clips from different voice models can lead to sudden volume jumps or awkward transitions. | Apply automated audio mastering via FFmpeg: EBU R128 loudness normalization ($-16\text{ LUFS}$ target), dynamic range compression, and subtle 15ms crossfades between spoken turns. |
| **Model Hallucinations in Intermediate Scripts** | Free-form LLM generation may yield broken JSON formatting or missing properties. | Constrain generation using strict GBNF grammars and Pydantic schemas, ensuring the model only outputs valid, parseable script tokens. |

---

*Submitted for Hacktoberfest Hack Day Nagpur × Elevate IIITN (Qualifier Round — Technical Architecture Proposal).*
