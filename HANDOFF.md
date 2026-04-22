# Realm Labs Sales Deck: Session Handoff

You are picking up work on the Realm Labs RSAC 2026 sales deck. A previous Claude Code session on another machine handed off this brief. Read everything before starting. Your goal: continue where we left off, pushing to `main` which auto-deploys to Vercel.

---

## Project essentials

| Item | Value |
|---|---|
| GitHub repo | `git@github.com:awesbecher/Realm-Labs-New-Sales-Deck-Apr26.git` |
| Live URL | https://realm-labs-new-sales-deck-apr26.vercel.app/ |
| Default branch | `main` |
| Vercel | Auto-deploys on every push to `main`. Rebuild ~60s. No manual deploy step. |
| Entry point | `index.html` (single-file deck, ~4000 lines, 15 slides) |
| Assets | `assets/` (images, SVGs, fonts) |

---

## Step 1: Clone on the new machine

Pick whichever auth matches your machine:

**HTTPS (works anywhere, no SSH key needed):**
```bash
cd ~/Desktop
git clone https://github.com/awesbecher/Realm-Labs-New-Sales-Deck-Apr26.git
cd Realm-Labs-New-Sales-Deck-Apr26
git pull
```
When pushing, you'll need a GitHub personal access token (not password). Create one at https://github.com/settings/tokens/new with `repo` scope. Git will cache it after first use.

**GitHub CLI (cleanest for long-term):**
```bash
brew install gh
gh auth login
gh repo clone awesbecher/Realm-Labs-New-Sales-Deck-Apr26
cd Realm-Labs-New-Sales-Deck-Apr26
```

**SSH (if the key is already set up on this machine):**
```bash
cd ~/Desktop
git clone git@github.com:awesbecher/Realm-Labs-New-Sales-Deck-Apr26.git
cd Realm-Labs-New-Sales-Deck-Apr26
```

Confirm you're on the latest commit:

```bash
git log --oneline -5
```

You should see recent commits referencing slide edits.

---

## Step 2: Set up the preview server

`.claude/` is git-ignored. Create `.claude/launch.json` locally so the preview MCP tool can start the dev server:

```bash
mkdir -p .claude
cat > .claude/launch.json <<'EOF'
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "deck",
      "runtimeExecutable": "python3",
      "runtimeArgs": ["-m", "http.server", "8765"],
      "port": 8765
    }
  ]
}
EOF
```

Then start the preview using the `preview_start` MCP tool (name: `deck`). It will serve on port 8765.

---

## Step 3: Navigating the deck in preview

The deck is a single-file HTML with 15 slides, JS-controlled via an `.active` class on one slide at a time. To activate a specific slide for screenshotting:

```js
document.querySelectorAll('.slide').forEach(s => s.classList.remove('active'));
document.getElementById('s7').classList.add('active');
```

**Gotcha**: slides 4 and 5 have swapped internal IDs. In DOM order:
- Position 4 has `id="s5"` (Why Now, "The time to act is now")
- Position 5 has `id="s4"` (Social Proof, Uber/Anthropic/McKinsey)

The user-facing footer numbers (`01 / 15` through `15 / 15`) are correct in presentation order. This is a legacy artifact from a past reorder. Don't try to "fix" the IDs. That would cascade breakage.

Preview viewport: resize to **1440×900** (the deck's native design target) before screenshotting.

---

## Current state of work

### Recently shipped (on `main`)

1. **Tagline refresh**: "Runtime AI Trust, Security, and Observability" became "Runtime AI Security & Observability" everywhere.
2. **Slide 1 hero**: new `assets/hero-viz-v3.png` (high-res), radial mask fade on edges. Headline updated to `Ship AI / your customers trust.` (lowercase second line).
3. **Slide 2**: symmetrical tagged-bar readouts on both sides. Left: ERROR / REFUSED / 👎 / BLOCKED (alarm colors). Right: HALLUCINATES / DRIFTS / HARMS / WALKAWAY (DNI colors). Each side with italic caption.
4. **Slide 7 (DNI pillars)**: three distinct subtle gradients per pillar (purple, teal/green hero, cyan). Proof stats added below each pillar.
5. **Slide 9**: rewritten with agentic-AI use cases (tool authorization, agent identity, workflow audit trails).
6. **Slide 11**: rebuilt as 3-step origin story (pattern matching, then mechanistic interpretability, then Deep Neural Inspection). DNI architecture diagram dropped in as `assets/dni-architecture.png` inside a white card, vertically centered. Previous Figma text was cropped out.
7. **Slide 14 (Team)**: killed circular avatar bubbles. Now 120×150 rounded-rectangle editorial portraits in horizontal cards (portrait beside name/role/bio). Advisor/investor footer box spaced properly.
8. **Slide 15 (CTA)**: killed the "60 minutes. Live demo. Your AI stack." caption. Period added to headline.

### In-progress (pushed to main; ready for you to continue)

**Typography floor pass**, partially done. Target floors per user spec:
- Body text (paragraphs, bios, descriptions): **≥ 0.875rem (14px)**
- Caption/UI labels (eyebrows, footer copy, tags, slide numbers, mono badges): **≥ 0.75rem (12px)**

**Already bumped** (~11 of ~25 total):
- `.slide-number`, `.footer-copy`, `.eyebrow` (clamp min), `.pill`, `.stats-bar` (clamp min)
- `#s2 .marker` (0.62 to 0.72, min-width 110px to fit HALLUCINATES), `#s2 .signal-caption` (0.78 to 0.875)
- `#s3 .arch-num` (0.68 to 0.75)
- `#s4 .cust-badge`, `#s4 .forrester .q-attr` (both 0.7 to 0.75)
- `#s7 .dni-proof-lbl` (0.62 to 0.75)
- `#s8 .integrations .lbl` (0.7 to 0.75)
- `#s9 .uc .bot` (0.78 to 0.875)
- `#s10 .bottom-stripe .src` (0.72 to 0.75)

**Remaining bumps** (search by selector in `index.html`, since line numbers may shift):

| Selector | Current | Target | Floor |
|---|---|---|---|
| `.eyebrow` `font-size: 11px !important` (late cascade override) | 11px | 12px | caption |
| `.nav-hint` | 0.68rem | 0.75rem | caption |
| `#s12 .bottom-row .chips span` | 0.65rem | 0.75rem | caption |
| `#s13 .opt .default-chip` | 0.62rem | 0.75rem | caption |
| `#s13 .opt .dep-sub` | 0.82rem | 0.875rem | body |
| `#s13 .enterprise-bar` | 0.78rem | 0.875rem | body |
| `#s14 .footer-lbl` | 0.7rem | 0.75rem | caption |
| `#s15 .chip` | 0.72rem | 0.75rem | caption |
| `#s15 .cta-meta` | 0.72rem | 0.75rem | caption |
| `#s15 .proof-strip .proof-lbl` | 0.66rem | 0.75rem | caption |
| "READABILITY SWEEP" block (around line 2839) | multiple at 0.72rem | 0.75rem | caption |

After finishing the sweep, commit as a single commit titled something like `Typography floor pass: enforce 14px body / 12px caption minimums`. Verify in preview that all slides still lay out correctly (nothing overflows, nothing breaks visually) before pushing.

### Refinement backlog (user has approved direction but not specific edits)

1. **Density pass** on Slides 3, 10, 12. These are the most text-heavy. Slide 12 especially: 4 standards cards plus a "Why DNI not LLM-as-judge" panel plus a "Your policy codified" panel is too much for one slide. DO NOT do this without user input on what to cut; it needs narrative judgment.
2. **Slide 5 card differentiation**: "Lost Sale / Runaway Cost / EU AI Act" all use the same green gradient for the title. Could differentiate by impact category.
3. **Visual consistency audit**: card shadows/borders vary across slides. After the current wave of edits, some slides feel "newer" than others. Standardize shadow tokens, border treatments, eyebrow spacing.
4. **Proof trail**: every stat in the deck (<15ms, 10K+ signals, 10%/90%, etc) should have a source or reason to be believed. Good for the "read" version.

### Known gotchas

- **Slide 11 diagram** sits in a white card on a dark slide. Intentional, because the Figma-exported diagram has dark text on a light background. Do not try to make it work on dark bg.
- **`.claude/launch.json` is gitignored**. Recreate per the setup above.
- **Hero image** `hero-viz-v3.png` is 12MB. Don't re-export at higher res.

---

## Conventions (user enforces these strictly)

### Writing style
- **NEVER use the em-dash character anywhere**. Not in commit messages, not in documents, not in code comments, not in slide copy, not in conversation. Use periods, commas, colons, semicolons, or parentheses instead. Zero exceptions. This is a hard rule the user enforces every time.
- Be direct and concise. No filler. No "Great question!" No excessive caveats.
- Structured output (tables, bullet lists) when organizing. Prose when drafting copy.

### Commits
- Style: descriptive first line, optional body with per-slide callouts. Look at `git log -5` for examples.
- Use HEREDOC for multi-line messages (avoids quoting issues):
```bash
git commit -m "$(cat <<'EOF'
Short descriptive first line.

Body lines describing what changed per slide.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```
- Always include the `Co-Authored-By:` footer.
- Commit only what changed. Never stage `.claude/` (gitignored). Never stage `.vercel/`.

### Push workflow
- Edit, verify in local preview (screenshot), commit, `git push origin main`.
- Vercel picks up the push and redeploys within ~60s. No manual deploy.
- Never force push. Never skip hooks.
- Test the live URL after deploy if changes are critical.

### Edit hygiene
- Always `Read` a file before `Edit`ing it (tooling enforces this).
- Prefer `Edit` for existing files. `Write` only for new files.
- Never create files unless needed for the task.

---

## How to verify changes (the workflow)

After editing source code, before committing:

1. Ensure preview is running (`preview_start` with name `deck`).
2. Reload: `preview_eval` with `location.reload()`.
3. Navigate to affected slide via the `.active` class trick above.
4. `preview_screenshot` to see the result.
5. If anything overflows, looks wrong, or breaks, iterate on the source and re-verify.
6. Only commit once it looks right.

Never skip verification for visual changes. Type-checking and tests verify correctness, not feature correctness. If you can't verify, say so explicitly rather than claiming success.

---

## About the user

**Wes Becher**, GTM Advisor / Fractional CRO for Realm Labs (among other clients). Account `awesbecher@gmail.com`.

- Role-appropriate framing: senior security buyer (CISO, VP Security) is the deck's audience unless specified otherwise.
- Thinks in: ICP, then pain, message, motion, pipeline, close.
- Short tasks get short answers. Deep strategy gets thorough treatment.
- Interrupts frequently with new asks mid-task. Finish the current edit, then address new input. Don't restart.

---

## First actions when you pick this up

1. `git pull origin main` to sync.
2. Skim `git log --oneline -15` to see what shipped.
3. Read this file (`HANDOFF.md`) in full.
4. Start the preview server.
5. Finish the **typography sweep** remaining bumps (the table above). Single commit.
6. Push.
7. Ask user which backlog item to tackle next.

Do not start on the density pass (#1 in backlog) without the user explicitly directing you to. It requires their narrative decisions per slide.
