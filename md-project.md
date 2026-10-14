# PATCHER
### Browser-Based AI Code Patcher & Diff Engine
### Living Design Document (LDD) V5

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
* **Mobile Wrap (Mode B Only):**  
  In Mode B planning & chat turns, break  
  lines manually (<35-40 chars) to prevent  
  mobile clipping. Mode A (chat planning  
  and patch code blocks) is exempt.  
* **Pathway Declaration:** At session start /  
  Turn 1 Inbound Check, the LLM must declare  
  the active delivery pathway (Mode A default  
  or Mode B) and state it will proceed under  
  that mode until explicitly redirected.  
* **LDD Version Integer Bump:** Any edit to a  
  Living Design Document strictly increments  
  its integer version (`V(N)` → `V(N+1)`).  
  The root filename remains unversioned  
  (`md-project.md`).  
* **Version Ceiling Guard:** Submodules must  
  remain within pre-1.0 (`v0.x.x`). LLMs are  
  strictly barred from bumping any version  
  to `v1.0.0+` without explicit PL sign-off.  

#### Delivery Pathways (Mode A vs Mode B)

##### Mode A: Traditional Patch Delivery (Chat LLM)
* **Automated Regex Engine Target:** Patches are parsed directly by an automated software patcher (this very application). Byte-for-byte fidelity is mandatory. Any whitespace, indentation, or character hallucination causes catastrophic failure.
* **Unified Multi-File Delivery:** Deliver all patches across all modified files in a single unified markdown response.
* **Target File Demarcation & Ordering:** Every file segment MUST begin with:
  `### TARGET FILE: path/to/file.ext`
  or for dynamic renaming during patching:
  `### TARGET FILE: path/to/old_file.ext -> path/to/new_file.ext`
  In multi-file deliveries, target file segments must be ordered strictly alphabetically by target source filename.
* **Strict Sequential Patch Headers:** Explicitly state patch index and total count: `Patch X/Y`.
* **Literal Contiguous Matching (Zero Shortcuts):** Matches are strictly applied via character-by-character regex slices. `Find`, `Find start`, and `Find end` blocks must be uninterrupted, contiguous slices of literal code. Never bridge code with ellipses (`...`, `...and...`, or `// rest of code`).
* **Patch Methods (Standard vs. Range):**
  * **Method 1: Standard Exact Match (Mandatory Default, 1–15 lines):** Replaces exact matching block using `Find` and `Replace with`. Required default for all code updates.
  * **Method 2: Range Match with Spatial Anchors (Strictly for Complete Block Rewrites):** Reserved exclusively for total block rewrites or deletions. Must NEVER be used to bridge distant edits.
    * **Engine Cursor Mechanics:** Locates `Find start`, sets search pointer to `cursor = start_match.index + start_match.length`, and searches forward for `Find end`. Replaces `[start_match.start() ... end_match.end()]` inclusive.
    * **Zero-Intersection Rule:** `Find end` must exist strictly downstream of `Find start`. `Find end` (and any substring thereof) must NEVER appear inside `Find start`.
    * **Minimal Boundary Span (2–4 Lines Mandate):** `Find start` and `Find end` are spatial boundary anchors only, NOT code payloads. Each anchor must be strictly 2–4 unique lines. NEVER paste the replacement body inside `Find start`.
* **Self-Referential Escaping Mandate (Meta-Patching Rule):**
  * When writing patches targeting Patcher's own code or markdown examples, use 4 backticks (` ```` `) for the outer fence enclosing any block that contains inner triple backticks.
  * Inside `index.html` UI markup (such as dialogues or tutorials), always use the HTML entity `&#96;&#96;&#96;` instead of raw backticks to guarantee universal parser safety and prevent premature block closure.
* **Anti-Drift Patch Constraints (Zero Tolerance):**
  * **Surgical Atomicity Mandate:** Disconnected edits separated by >3 lines must be split into separate micro-patches (`Patch 1/N`, `Patch 2/N`). Bridging untouched lines across disconnected edits is prohibited.
  * **Engine First-Match Defense:** JavaScript patch engines use `String.prototype.replace()`, replacing only the first occurrence. `Find` blocks MUST include 1–2 unique enclosing lines of context (function headers, distinct parent statements) to prevent collision with identical lines earlier in the file.
  * **Verbatim Grounding & Whitespace Fidelity:** Anchors must be copied byte-for-byte from intake files. Zero whitespace, tab, or indentation reformatting.
  * **Single-Scope Boundary Mandate:** Every patch must be scoped strictly to a single function, method, object, or CSS rule set.
  * **Zero Unmodified Pass-Through:** A patch payload (`Replace with`) must NEVER contain untouched sibling code simply to bridge two separate changes. Split into sequential patches instead.
  * **Payload Ceiling:** Standard matches must remain 1–15 lines; Range match replacement bodies must never exceed 40 lines.
* **Post-Patch Manifest Declaration (Strictly Alphabetical):** Every delivery must conclude with:
  1. Target declaration: `These edits apply to: [list]` (alphabetical)
  2. Alphabetical list of updated files & versions.
  3. Active Manifest Roster in strict alphabetical order marked `[UPDATED]` or `[unchanged]`.

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

### TARGET FILE: js-app.js

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
    1. Target file declaration: `These edits apply to: [list]` (alphabetical)  
    2. Alphabetical list of updated files & bumped versions.  
    3. Mode A: Applied patches ready for extraction.  
       Mode B: Direct Drive link to `update_vX.X.X.zip`.  
    4. Active Manifest Roster in strict alphabetical order showing all modules marked:  
       `[UPDATED]`, `[unchanged]`, or `[unchanged - floor]`.  

---

### 4. GLOBAL UI & NAVIGATION

* **Palette:** CSS Variable-based tokens:  
  `--bg-main`, `--bg-panel`, `--text-main`,  
  `--text-muted`, `--border-color`, `--accent-color`.  
  Hardcoded hex values outside root definitions are strictly barred.  
* **Theming:** Dark mode toggled via `[data-theme="dark"]`  
  on root element, persisted in `localStorage`.  
* **Buttons:** Material 3 pill tokens (`--radius-pill: 9999px`).  
* **Icons & Feedback:** Inline SVGs only (`ICONS` dictionary).  
  CSS keyframe pulse skeletons and circular spinners for async tasks.  
* **Responsive Layout:** Responsive CSS Grid & Flexbox.  
  Collapses to single-column stacking under 768px viewports.  

---

### 5. SYSTEM & ARCHITECTURE CONSTRAINTS

* **Application Summary:** Patcher is a zero-dependency, single-file browser workbench that automates applying AI-generated code diffs to project source files directly in-memory. Users upload working project files, paste LLM markdown containing standard or range patches, and Patcher executes exact byte-level find-and-replace routines, displaying real-time previews, trace navigation, and bundle packaging.
* **The 3-Module Engine Flow:**
  * **Module 1 (Input):** Multi-file intake supporting up to 25 files via drag-and-drop or system file picker. Includes responsive touch/scroll snap carousel tabs with strict horizontal overflow containment (`min-width: 0`), and live sandboxed iframe previews.
  * **Module 2 (Engine):** Direct textarea `#instructions-input` with auto-clipboard ingestion, debounced parsing, and dual-mode regex evaluation: Standard (`Find`) and Range (`Find start` + `Find end`). Features dynamic file renaming (`old -> new`) and bi-directional click-to-trace line navigation for both applied and missing patches.
  * **Module 3 (Output):** Smart patched output rendering, responsive tab switching, editable filename synchronization, single-file downloads, clipboard copying, and PKZip archive generation respecting renamed filenames.
* **MD Viewer Multi-Panel Dock:** Expandable reference dock with up to 5 dynamic file slots rendering markdown via CDN Marked.js (`marked.parse()`). Offers auto-appending next-slot readiness, secondary "Replace" button switching, and direct header toggle triggering immediate native file picking.
* **PKZip Generation Engine:** Embedded pure vanilla ZIP builder (`CRC_TABLE`, `calculateCrc32()`, local and central directory headers). No external libraries (e.g. JSZip) allowed.
* **GitHub LDD Fetcher & Template Conventions:**
  * Queries GitHub API (`/repos/spamfan/patcher/contents/`) for downloadable LDD rulebooks.
  * The engine regex evaluates `/ldd(\d*)\.md/i`.
  * Public template files uploaded to root must strictly match `ldd{N}.md` (e.g., `ldd6.md`).
* **EOF Whitespace Safety:** Avoid anchoring against raw trailing EOF lines (e.g. `</html>`) due to platform line-break discrepancies (`\r\n` vs `\n`).

---

### 6. BASELINE FLOOR & FILE TREE

#### Version Floor
* **App (Patcher):** `v0.2.6`  
* **`index.html`:** `v0.2.6`  
* **`md-project.md`:** `V5`  

#### File Tree
* `index.html`
* `ldd6.md`
* `md-project.md`

---

### 7. SUB-SPOKE DIRECTORY & CONTEXT GUARD

Patcher is a single-file application (`index.html`). All core logic, stylesheets, and templates reside directly at repository root.

* **Pre-Flight File Lockout:** If `index.html` or `md-project.md` are referenced but unattached, reply ONLY with the required intake file list and halt.
* **Context Guard:** Whenever editing `index.html`, ensure the active version badge (`<span class="version-badge">vX.X.X</span>`) is bumped monotonically.