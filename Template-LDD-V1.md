# [PROJECT NAME]
### [Subsystem / Track]
### Living Design Document (LDD) V1

---

### 1. WATERFALL PROTOCOL & DUAL DELIVERY

#### The Loop
1. **Sprint Plan (SP):** Scope & target files.  
   *End prompt:* "Reply 'yes' to proceed to Tech Spec."  
2. **Tech Spec (TS):** Functions & logic diffs.  
   *End prompt:* "Reply 'yes' to proceed to Execution."  
3. **Execution:** Authorized delivery via Mode A or Mode B.  

#### Gate Rules
* **Prompted Handshake:** Gates advance *only*  
  in response to an active LLM gate prompt.  
  Unprompted "yes" never advances a gate.  
* **Triggers:** `yes`, `proceed`, `do it`, `patches`.  
* **Hard Stop:** Halt after every phase.  
  Never combine phases in one turn.  
* **Scope & Tone:** Bullets only. No fluff.  
  Declare file paths. No unasked refactors.  
* **Mobile Wrap (Mode B / Chat Only):**  
  In planning & chat turns, break lines  
  manually (<35-40 chars) to prevent  
  mobile horizontal clipping. Mode A  
  patch code blocks are strictly exempt  
  to protect regex whitespace fidelity.  

#### Delivery Pathways (Mode A vs Mode B)

##### Mode A: Traditional Patch Delivery (Chat LLM)
* **Automated Regex Engine Target:** Patches are parsed directly by an automated software patcher. Byte-for-byte fidelity is mandatory. Any whitespace, indentation, or character hallucination causes catastrophic failure.
* **Unified Multi-File Delivery:** Deliver all patches across all modified files in a single unified markdown response.
* **Target File Demarcation:** Every file segment MUST begin with:
  `### TARGET FILE: path/to/file.ext`
* **Strict Sequential Patch Headers:** Explicitly state patch index and total count: `Patch X/Y`.
* **Literal Contiguous Matching (Zero Shortcuts):** Matches are strictly applied via character-by-character regex slices. `Find`, `Find start`, and `Find end` blocks must be uninterrupted, contiguous slices of literal code. Never bridge code with ellipses (`...`, `...and...`, or `// rest of code`).
* **Patch Methods (Standard vs. Range):**
  * **Method 1: Standard Exact Match (1–15 lines):** Replaces exact matching block using `Find` and `Replace with`.
  * **Method 2: Range Match with Spatial Anchors (Refactoring large blocks):**
    * **Engine Cursor Mechanics:** The patcher locates `Find start`, sets search pointer to `cursor = start_match.index + start_match.length`, and searches forward for `Find end`. Replaces `[start_match.start() ... end_match.end()]` inclusive.
    * **Zero-Intersection Rule:** `Find end` must exist strictly downstream of `Find start`. `Find end` (and any substring thereof) must NEVER appear inside `Find start`.
    * **Minimal Boundary Span (2–4 Lines Mandate):** `Find start` and `Find end` are spatial boundary anchors only, NOT code payloads. Each anchor must be strictly 2–4 unique lines. NEVER paste the replacement body inside `Find start`.
* **Post-Patch Manifest Declaration:** Every delivery must conclude with:
  1. Target declaration: `These edits apply to: [list]`
  2. Updated files & versions list.
  3. Active Manifest Roster marked `[UPDATED]` or `[unchanged]`.

###### Mode A Template Reference

*Sample Standard Patch Format:*
```
Patch 1/2
Find
` ` `html
<div class="old-header">Old Header</div>
` ` `
Replace with
` ` `html
<div class="new-header">New Header</div>
` ` `
```

*Sample Range Patch Format:*
```
Patch 2/2
Find start
` ` `html
<div class="card-container">
` ` `
Find end
` ` `html
</div><!-- end card -->
` ` `
Replace with
` ` `html
<div class="card-container">
    <p>New Card Content</p>
</div><!-- end card -->
```

*Sample Multi-File Demarcation:*
```
### TARGET FILE: styles.css

Patch 1/1
Find
...
Replace with
...

### TARGET FILE: js/app.js

Patch 1/1
Find start
...
Find end
...
Replace with
...
```

##### Mode B: Spark Agent (Workspace & Drive)
* **Zero Chat Patches:** Mode A and Mode B are mutually exclusive. Zero code patches, blocks, or full file dumps in chat.
* **Workspace Execution:** Synthesize files directly in sandbox; validate headlessly (`node --input-type=module -c`).
* **Google Drive Isolation Boundary:** Restricted **strictly** to folder `just Gemini stuff`. Never mutate anything outside.
* **Zip Deliverable:** Package modified files into `just Gemini stuff/update_vX.X.X.zip`.
* **Chat Output Limits:** Restricted strictly to direct Drive link, test status, and Manifest Roster.

---

### 2. INBOUND CHECKS & MEDIA RULES

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

---

### 3. WORKSPACE, DRIVE & EXECUTION SIGN-OFF

* **Drive Isolation (Mode B Only):** The agent  
  is authorized **exclusively** to create files  
  inside the Google Drive folder: `just Gemini stuff`.  
  Touching or modifying anything outside is forbidden.  
* **Surgical Edits:** Targeted lines only.  
  Unchanged lines must remain byte-for-byte  
  identical across both delivery modes.  
* **Execution Sign-Off Requirements:**  
  * Every delivery turn must conclude with:  
    1. Target file declaration: `These edits apply to: [list]`  
    2. List of updated files & bumped versions.  
    3. Mode A: Applied patches ready for extraction.  
       Mode B: Direct Drive link to `update_vX.X.X.zip`.  
    4. Active Manifest Roster showing all modules marked:  
       `[UPDATED]`, `[unchanged]`, or `[unchanged - floor]`.  

---

### 4. GLOBAL UI & NAVIGATION

* **Design System / Palette:** [Define  
  core brand colors, backgrounds, surfaces].  
* **Component Tokens:** [Define button  
  tokens, radius, elevation guidelines].  
* **Icons & Feedback:** Inline SVGs only.  
  CSS spinners on async actions.  
* **Overlay / Modal Dismissal:** [Define  
  dismissal rules: backdrop, Esc, close btn].  
* **History & Navigation:** [Define pushState,  
  routing, or deep-linking behaviors].  

---

### 5. HARDWARE & SYSTEM CONSTRAINTS

* **Target Environment:** [Define display  
  resolutions, browsers, or kiosk limits].  
* **Domain Terminology:** [List any strictly  
  required or forbidden domain keywords].  
* **Cache-Busting Imports:** Pin query  
  strings if required: `?v=X.X.X`  
* **Auth & Security:** [Define local test  
  credentials, auth gates, or bypass tokens].  

---

### 6. BASELINE FLOOR & FILE TREE

#### Version Floor
* **App Baseline:** `v0.1.0`  
* **`index.html`:** `v0.1.0`  
* **`styles.css`:** `v0.1.0`  
* **`app.js`:** `v0.1.0`  

#### File Tree
```

---

### 7. SUB-SPOKE DIRECTORY & CONTEXT GUARD

When modifying specific subsystems, attach  
the corresponding spec from `docs/`:  
* `docs/subsystem-a.md` -> `js/module-a.js`  
* `docs/subsystem-b.md` -> `js/module-b.js`  

* **Context Guard:** If a task touches these  
  files and the spec is unattached, the LLM  
  must ask the user to attach it before starting.
