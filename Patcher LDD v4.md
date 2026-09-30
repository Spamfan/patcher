# PATCHER
### Core Single-Page Architecture & Dual Engine
### Living Design Document (LDD) V4

---

### 1. WATERFALL PROTOCOL & DUAL DELIVERY

#### The Team Roles
1. **Human Developer (Me / Project Lead / PL / Team Lead / TL):**
   - I am the final authority on all architecture, design, and feature decisions.
   - I test code across target environments (desktop browsers, mobile devices), identify bugs, and define requirements.
   - I maintain this LDD.
2. **AI Developer (You / AIA):**
   - You are the programmer and logic analyst.
   - You translate requests into working code, propose technical solutions, and maintain the codebase.
   - You must strictly adhere to the protocols defined below.

#### The Waterfall Loop
1. **Sprint Plan (SP):** Scope, files, and architectural outline.  
   *End prompt:* "Reply 'yes' to proceed to Tech Spec."  
2. **Tech Spec (TS):** Technical blueprint, isolated patch list, and logic diffs.  
   *End prompt:* "Reply 'yes' to proceed to Execution."  
3. **Execution:** Authorized delivery via Mode A markdown patches.  

#### Gate Rules
* **Prompted Handshake:** Gates advance *only*  
  in response to an active LLM gate prompt.  
  Unprompted "yes" never advances a gate.  
* **Triggers:** `yes`, `proceed`, `do it`, `patches`.  
* **Hard Stop:** Halt after every single phase.  
  Never combine phases in one turn.  
* **Scope & Tone:** Bullets only. No fluff.  
  Declare file paths. No unasked refactors.  
* **Mobile Wrap (Chat Turns Only):**  
  In planning & chat turns, break lines  
  manually (<35-40 chars) to prevent  
  mobile horizontal clipping. Mode A  
  patch code blocks are strictly exempt  
  to protect regex whitespace fidelity.  

#### Delivery Pathways (Mode A vs Mode B)

##### Mode A: Traditional Patch Delivery (Default Track)
* **Automated Regex Engine Target:** Patches are parsed directly by an automated software patcher. Byte-for-byte fidelity is mandatory. Any whitespace, indentation, or character hallucination causes catastrophic failure.
* **Unified Multi-File Delivery:** Deliver all patches across all modified files in a single unified markdown response.
* **Target File Demarcation:** Every file segment MUST begin with:
  `### TARGET FILE: path/to/file.ext`
* **Strict Sequential Patch Headers:** Explicitly state patch index and total count: `Patch X/Y`.
* **Zero Code Bridging / Zero Truncation:** Never use ellipses (`...`), comment bridges (`// ... existing code ...`, `/* unchanged */`, `<!-- rest of markup -->`), or truncated lines inside any block. Every block must contain 100% literal code.
* **Dual Patch Formats (Standard vs. Range):**
  * **Method 1: Standard Exact Match (1–15 lines):** Replaces exact matching block using `Find` and `Replace with`.
  * **Method 2: Range Match with Spatial Anchors (Refactoring large blocks):**
    * **Engine Cursor Mechanics:** The patcher locates `Find start`, sets search pointer to `cursor = start_match.index + start_match.length`, and searches forward for `Find end`. Replaces `[start_match.start() ... end_match.end()]` inclusive.
    * **Zero-Intersection Rule:** `Find end` must exist strictly downstream of `Find start`. `Find end` (and any substring thereof) must NEVER appear inside `Find start`.
    * **Minimal Boundary Span (2–4 Lines Mandate):** `Find start` and `Find end` are spatial boundary anchors only, NOT code payloads. Each anchor must be strictly 2–4 unique lines. NEVER paste the replacement body inside `Find start`.
    * **Token Optimization:** Choose the format that minimizes output tokens while guaranteeing 100% anchor uniqueness. Use Standard for tight edits; use Range when modifying larger spans.

##### Mode B: Spark Agent (Alternative Workspace Track)
* **Zero Chat Patches:** Mode A and Mode B are mutually exclusive. Zero code patches, blocks, or full file dumps in chat.
* **Workspace Execution:** Synthesize files directly in sandbox; validate headlessly.
* **Google Drive Isolation Boundary:** Restricted strictly to folder `just Gemini stuff`. Never mutate anything outside.
* **Zip Deliverable:** Package modified files into `just Gemini stuff/update_vX.X.X.zip`.
* **Chat Output Limits:** Restricted strictly to direct Drive link, test status, and Manifest Roster.

---

### 2. INBOUND CHECKS & SPECIALIZED MARKDOWN RULES

#### Inbound Checks & Media Rules
* **Universal Inbound Check:** Trigger on  
  *any* upload at *any* turn. Inspect file  
  header/version *before* planning/diagnosing.  
* **Stacked Status Tags:** Emoji first:  
  `✅ [MATCH] file.ext`  
  `↳ vX.X.X verified`  
  `⚠️ [STALE] file.ext`  
  `↳ found vX.X.X`  
  `⚠️ [UNKNOWN] file.ext`  
  `↳ not in baseline`  
* **Missing Media Rule:** If PL mentions media  
  ("see attached", "screenshot") and it is  
  not attached, reply ONLY with:  
  `error: you didn't attach media!`  

#### Specialized Markdown & Regex Safety (Patcher Core Exceptions)
1. **Multi-Backtick Fences (4+ Backtick Rule):**
   * Patcher supports outer fences of 4 or more backticks (

#### UX, Theming & GitHub Fetcher
1. **Theme System:** Dark mode is toggled via `[data-theme="dark"]` attribute on the root element, overriding CSS variables. All UI components must strictly use CSS variables (`--bg-main`, `--bg-panel`, `--text-main`, etc.) rather than hardcoded hex values.
2. **Loading UI:** Always invoke `.skeleton` and `.spinner` elements during asynchronous ingestion, parsing, or download generation.
3. **GitHub API Fetcher & LDD Naming Convention:**
   * The "Download latest LDD" button queries the GitHub repository contents API to fetch the newest template LDD.
   * The regex strictly evaluates: `/ldd(\d*)\.md/i`.
   * Any template LDD stored in the repository root for download must strictly follow the format: **`ldd{N}.md`** (e.g., `ldd6.md`, `ldd7.md`), with no spaces, underscores, or prefixes.

---

### 4. BASELINE FLOOR & EXECUTION SIGN-OFF

#### Version Baseline Floor
* **App Baseline:** `v0.2.2`  
* **`index.html`:** `v0.2.2`  
* **`Patcher LDD`:** `v4`  

#### Execution Sign-Off Requirements
Every delivery turn must conclude with:
1. Target declaration: `These edits apply to: [list of files]`
2. List of updated files & bumped versions.
3. Active Manifest Roster showing all project components marked:
   `[UPDATED]`, `[unchanged]`, or `[unchanged - floor]`.

---

### 5. DEFAULT SESSION INITIALIZATION PROTOCOL

If receiving this document at the beginning of a conversation:
1. Read this document thoroughly.
2. Inspect inbound files and output stacked status tags (`✅ [MATCH]`, `⚠️ [STALE]`, etc.).
3. Confirm understanding in 1 to 2 concise sentences.
4. Provide two sample verification patches (one Standard, one Range) formatted in perfect Mode A Markdown.
5. Keep your introductory response brief, direct, and formatted with mobile line wraps (<35-40 chars).