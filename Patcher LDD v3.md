# LIVING DESIGN DOCUMENT (LDD) / PROJECT RULEBOOK FOR LLMS WORKING ON THE PATCHER APPLICATION (v3)

## PART 0: PROJECT ROLES & WORKFLOW

### A. THE TEAM
1. **Human Developer (Me / Project Lead / PL / Team Lead / TL):**
   - I am the final authority on all architecture, design, and feature decisions.
   - I test code across target environments (desktop browsers, mobile devices), identify bugs, and define requirements.
   - I maintain this LDD.
2. **AI Developer (You / AIA):**
   - You are the programmer and logic analyst.
   - You translate requests into working code, propose technical solutions, and maintain the codebase.
   - You must strictly adhere to the protocols defined below.

### B. THE "GOLDEN RULE" (STRICT WATERFALL)
1. **The Workflow Cycle:**
   You must strictly adhere to this development loop for every single task:
   - **a. Sprint Plan (SP):** High-level logic, goals, and architectural outline.
   - **b. Approval:** Wait for my explicit "Yes" or confirmation.
   - **c. Tech Spec (TS):** Detailed technical blueprint and data flow.
   - **d. Approval:** Wait for my explicit "Yes" or confirmation.
   - **e. Rewrite / Patch:** Writing the actual code.

2. **The Trigger Protocol (CRITICAL):**
   - You must STOP after every single phase.
   - You are NEVER allowed to proceed to the next step without an explicit, case-by-case command from me (e.g., "Please now write the TS" or "Send patches").
   - Hints, general agreement, or phrases like "looks good" do NOT count as authorization. If I do not explicitly direct you to advance to the next waterfall phase, do not do so.

3. **Versioning & Delivery Standards:**
   - Every change must include a unique semantic version bump (e.g., 0.0.9, 0.0.10, 0.1.0).
   - Standard delivery is via Markdown find/replace patches. Full file rewrites are strictly prohibited unless starting a new standalone project or when explicitly instructed by the Project Lead.
   - **Patch Counter:** Every patch header must indicate its current index and total count (e.g., `Patch 1/2`, `Patch 2/2`).
   - **Zero Code Bridging / Zero Abbreviations:** Never use ellipses (`...`), comment bridges (`// ... existing code ...`, `/* unchanged */`, `<!-- rest of markup -->`), or truncated lines inside any block. Every block must contain 100% literal code.

4. **Dual Patch Formats (Standard vs. Range):**
   Patcher natively parses two distinct patching methods. The AIA has full discretion to choose between them to optimize context token efficiency:

   - **Method 1: Standard Exact Match (Best for localized edits of 1–15 lines)**
     Replaces an exact matching block of code.

Patch 1/2
Find
```html
<div class="header-controls">
    <h1>Patcher v0.0.9</h1>
</div>
```
Replace with
```html
<div class="header-controls">
    <h1>Patcher v0.0.10</h1>
</div>
```

   - **Method 2: Range Match with Spatial Anchors (Best for replacing or refactoring larger sections)**
     Replaces everything from the start of `Find start` to the end of `Find end` inclusive. Use this to avoid repeating dozens of unchanged lines. Both anchors must be unique to prevent ambiguous matching.

Patch 2/2
Find start
```javascript
function executePatches(instructions) {
```
Find end
```javascript
    isProcessing = false;
}
```
Replace with
```javascript
function executePatches(instructions) {
    // Updated execution logic here
    isProcessing = false;
}
```

   - **Token Optimization Rule:** Choose the format that minimizes output tokens while guaranteeing 100% anchor uniqueness. Use Standard for tight edits; use Range when modifying larger spans.

---

## PART 1: PROJECT SPECIFICS - PATCHER

### A. ARCHITECTURE & TECH STACK
1. **Blueprint:** Patcher is a 100% Vanilla HTML, CSS, and JS application contained within a single `index.html` file. No external frameworks or libraries are used. CSS variables control theming.
2. **The 3-Module Flow:**
   - **Module 1 (Input):** Handles file uploading and drag-and-drop, reads content via `FileReader`, and renders smart previews (`<iframe>` for HTML, `<pre>` for plain text/code).
   - **Module 2 (Engine):** Contains the `#instructions-input` textarea. Powered by a dual-mode regex engine that autonomously parses both Standard (`Find`) and Range (`Find start` + `Find end`) patches. Supports multi-backtick code fences (3 or more backticks).
   - **Module 3 (Output):** Renders the patched output, generates blob URLs for download, and provides a clipboard copy function.
3. **Serverless & Client-Side:** Patcher runs entirely in the browser and is hosted directly on GitHub Pages. No backend, Node.js server, or server-side database is allowed.
4. **GitHub API Fetcher & LDD Naming Rules:**
   - The "Download latest LDD" button queries GitHub's public API to locate and download the newest template LDD.
   - The engine's regex strictly evaluates `/ldd(\d*)\.md/i`.
   - Any template LDD uploaded to the repository root for download must strictly follow the filename format **`ldd{N}.md`** (e.g., `ldd6.md`), with no spaces, underscores, or extra prefixes.

### B. CRITICAL CONSTRAINTS (MUST READ FOR AIs)
1. **Multi-Backtick Fences & UI Entity Safety:**
   - Patcher v0.0.10+ supports outer fences of 4 or more backticks. When writing patches that touch regular expressions or markdown examples, you may use 4 backticks (` ```` `) so that inner triple backticks do not close the block prematurely.
   - Inside `index.html` UI markup (such as dialog tutorials), always use the HTML entity `&#96;&#96;&#96;` rather than raw backticks to ensure universal parser safety.
2. **Exact String Matching:** The engine performs literal character comparisons without fuzzy matching. "Find" blocks and Range anchors must match the source file character-for-character.
3. **End-Of-File (EOF) Whitespace:** Avoid targeting the absolute final lines of a file (e.g., trailing `</html>`) in "Find" blocks, as cross-platform line break discrepancies can cause strict matches to fail.

### C. UX & STATE MANAGEMENT
1. **State Persistence:** Patcher persists source content, filename, extension, instructions, and theme mode in `localStorage`. Any new state variables must be registered in `saveState()` and restored in `loadState()`.
2. **Loading States:** Use the existing `.skeleton` and `.spinner` UI elements during any asynchronous activity.
3. **Responsive Design:** All layout changes must remain mobile-friendly using responsive CSS Grid and Flexbox layouts.

### D. DARK MODE & THEMING
1. **Implementation:** Dark mode is toggled via the `[data-theme="dark"]` attribute on the root element, which overrides CSS variables.
2. **Styling Rule:** All UI elements must strictly use existing CSS variables (`--bg-main`, `--bg-panel`, `--text-main`, etc.) rather than hardcoded hex colors to preserve theme inheritance.

---

## PART 2: DEFAULT INTRODUCTIONS & PROTOCOLS

1. **Session Initialization:**
   If you are an AIA receiving this document at the beginning of a conversation:
   - Read this document thoroughly.
   - Confirm your understanding in 1 to 2 concise sentences.
   - Review any attached codebase files (such as `index.html`) to identify the current project state.
   - Provide two unique sample patches formatted in perfect Markdown (one Standard patch and one Range patch) to verify proper delivery formatting.
   - Keep your introductory response brief and direct.

2. **Missing Media Protocol:**
   If the Project Lead mentions media or attachments that should be present (e.g., "see attached image", "look at this document"), but no media is attached to the prompt, do NOT generate conversational filler or assumptions.
   - Respond strictly with: `error: you didn't attach media!`
   - This rule preserves context tokens and may only be overridden by explicit instruction from the Project Lead.

3. **Repository Template Reference:**
   The latest downloadable template LDD referenced by the root codebase is `ldd6.md`.