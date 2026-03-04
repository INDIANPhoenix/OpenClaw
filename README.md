# Always-On OpenClaw Agent Team (24/7)

A practical implementation plan that *actually* gets better over time.

## What you’re building

An **always-on agent team** that:

- runs on a schedule (cron)
- self-heals (heartbeat)
- coordinates via the filesystem (markdown + json)
- improves through a disciplined “feedback → memory → load next run” loop

This is not “agents magically learning.” The **system** improves because **workspace files are loaded every session** (SOUL/AGENTS/USER/etc.), so the agent behavior compounds. ([OpenClaw][1])

---

## 0) Non-negotiables (before you start)

### A) Your “One Job per Agent” rule

Don’t assign 5 responsibilities to one agent.
Each agent must have:

- one job
- clear inputs
- clear outputs
- stop condition (when to stop / what is “done”)

### B) The “One Writer per File” rule

Design shared files so **only one agent writes** and many agents read.
This prevents coordination conflicts and silent corruption.

### C) You will run this like infrastructure

Agents are not prompts. They are **systems**:

- failures happen
- schedules drift
- tokens balloon
- dependencies break

Heartbeat + maintenance is part of the plan. ([OpenClaw][2])

---

## 1) Prerequisites (practical setup)

### Hardware options

Any always-on machine works:

- Mac mini / desktop PC / old laptop
- Linux VPS if you’re comfortable with server ops
- Windows + WSL works too (if you prefer)

Goal: **always on + stable internet + stable storage**.

### Communication channel

Use Telegram as your “control plane”:

- you review drafts
- you approve/reject
- you give feedback (which must be written to memory files)

Telegram streaming modes exist and can show partial replies as they generate. ([OpenClaw][3])

### Security boundary (super important)

Treat agents like new hires:

- give them their own workspace + keys
- never give access to your personal accounts by default
- share only what you want them to see

Also: be cautious with third-party “skills/extensions” ecosystems. There have been public reports of malicious add-ons in agent marketplaces; keep your system minimal and auditable. ([The Verge][4])

---

## 2) The core architecture (conceptual)

### The three-layer “Agent OS”

This is the most important mental model:

**Layer 1: Identity**

- SOUL.md (who the agent is)
- IDENTITY.md (quick card)
- USER.md (who it serves)

**Layer 2: Operations**

- AGENTS.md (startup routine + rules)
- HEARTBEAT.md (health checks + recovery playbook)
- role guides (only when patterns repeat)

**Layer 3: Knowledge**

- MEMORY.md (curated long-term memory)
- daily logs (raw)
- shared-context/ (cross-agent alignment)

OpenClaw’s workspace file map explicitly describes these standard files and that key ones load at the start of sessions. ([OpenClaw][1])

---

## 3) Visual architecture diagram (use your provided assets)

### Diagram A: “Always-on Agent Teams schedule”

**Use this image right after the Scheduling section.**
Caption: “Order matters: research first → downstream drafts after.”

- File: your schedule table screenshot

### Diagram B: “Agent team getting smarter over time”

**Use this image right after the Three-layer OS section.**
Caption: “Feedback enters the system → becomes files → loads next run.”

- File: your “Layer 1/2/3 + feedback + FS integration layer” image

### Diagram C: Telegram interface

**Use this image in the “Telegram as control plane” section.**
Caption: “No dashboard needed. Review + feedback lives in chat.”

If you want, I can also produce a “clean” version of the diagram (same content, minimal style). Just tell me your preferred style: dark / light / monochrome.

---

## 4) Directory structure (final target)

### Start small (Week 1 target)

You need only these at first:

- SOUL.md
- USER.md
- AGENTS.md
- (optional tiny) HEARTBEAT.md
- memory/ folder for daily logs

OpenClaw docs: these are standard workspace files and their purpose. ([OpenClaw][1])

### Scale to full multi-agent (Week 3–4 target)

Use this final structure as your north star:

- root workspace = main agent (Monica / “Chief of Staff”)
- /agents/* = specialist agents
- /intel/* = research outputs
- /shared-context/* = cross-agent alignment
- /memory/* = root long-term + operational logs

Important: keep sensitive info out of shared-context if it is loaded widely. (Your own policy.)

---

## 5) Step-by-step implementation plan (real-world)

### Phase 1 (Day 1): Install + one agent + one output

**Objective:** Monica runs, replies on Telegram, and writes one daily file.

**Steps**

1. Choose your “first boring job”
   Example: “Morning research summary” OR “Draft 3 tweets from provided intel.”
   Pick the one you repeat daily.

2. Create Monica’s identity layer

   - SOUL.md: role + vibe + principles (keep short)
   - USER.md: your preferences (style, constraints, timezone)
   - IDENTITY.md: quick card (optional but helpful)

3. Create AGENTS.md (root rules)
   Define:

   - startup read order
   - where to write daily logs
   - when to promote to MEMORY.md
   - safety rules (no secrets leakage)

OpenClaw explicitly documents that AGENTS.md/SOUL.md/USER.md are standard files and loaded each session. ([OpenClaw][1])

4. Create a single output contract
   For example, Monica must always produce:

   - “Deliverables”
   - “What I need from you”
   - “Next scheduled run”

**Success criteria**

- You can ask Monica on Telegram and get structured output.
- Monica writes a daily log entry.

### Phase 2 (Days 2–3): Add scheduling (cron) + delivery to Telegram

**Objective:** Monica wakes up automatically and posts results to you.

Cron is OpenClaw’s gateway scheduler and is the correct mechanism for “run every morning”. ([OpenClaw][5])

**Steps**

1. Decide one recurring schedule
   Example: every day 8:01 AM (your timezone)

2. Decide output destination

   - Telegram DM (recommended early)
   - Later: group/topic

3. Define “Run Prompt” content
   Keep it stable and simple:

   - “Do X using Y inputs”
   - “Write output to file Z”
   - “Send summary to Telegram”

**Success criteria**

- It runs without you triggering it.
- You receive the message in Telegram at expected time.

### Phase 3 (Week 1): Add memory discipline (this is the compounding engine)

**Objective:** Corrections stop repeating.

**Rules to implement**

1. Daily logs are raw and cheap

   - store every run’s notes
   - store your feedback verbatim

2. MEMORY.md is curated and expensive

   - only store rules that repeat
   - store “hard lessons” permanently
   - keep it short enough to remain useful

3. Promotion policy (very practical)

- If the same correction happens **2 times**, promote it to MEMORY.md.
- If it applies to multiple agents, promote it to shared-context/FEEDBACK-LOG.md later.

This aligns with the documented role of AGENTS.md and workspace-driven continuity. ([OpenClaw][1])

**Success criteria**

- You stop giving the same feedback repeatedly.
- Monica starts reminding herself via MEMORY.md rules.

### Phase 4 (Week 2): Add Dwight (research) + file handoff (filesystem integration)

**Objective:** Monica stops doing research herself; Dwight becomes upstream.

**Steps**

1. Create Dwight’s SOUL.md

   - strict: never fabricate
   - always cite sources
   - “signal over noise” ranking system
   - output format stable

2. Create Dwight’s output files

   - intel/DAILY-INTEL.md (human-readable)
   - intel/data/YYYY-MM-DD.json (structured source of truth)

3. Update Monica’s AGENTS.md

   - read intel/DAILY-INTEL.md before making decisions
   - do not redo research unless intel is missing/stale

**Success criteria**

- Dwight runs on schedule and updates intel file.
- Monica uses Dwight’s intel as input for decisions.

### Phase 5 (Week 3): Add Kelly + Rachel (content agents)

**Objective:** content drafting becomes downstream of intel.

**Steps**

1. Kelly reads intel/DAILY-INTEL.md only
   She does not research.
   She drafts content using your USER.md preferences.

2. Add specialist guides only when needed
   If you repeatedly correct:

   - tweet length
   - hooks
   - tone
     then create role-guides:
   - X-STYLE.md
   - X-FORMATS.md
   - X-EXAMPLES.md

3. Rachel mirrors the same pattern
   Same intel input, different output formats.

**Success criteria**

- You get daily drafts with minimal edits needed.
- Output matches your voice rules.

### Phase 6 (Week 3–4): Add Ross (engineering) + PR triage workflow

**Objective:** Ross produces actionable engineering work, not essays.

**Steps**

1. Define Ross’s “definition of done”
   Examples:

   - “Reject PR with reasons + suggested next step”
   - “List issues ranked by impact + effort”
   - “Propose patch plan”

2. Define safe access boundaries

   - what repos
   - what commands (if any)
   - what files are writable

**Success criteria**

- Ross produces concise PR decisions you can approve quickly.
- Fewer “hallucinated” technical claims because he is constrained.

### Phase 7 (After first failure): Implement HEARTBEAT self-healing

**Objective:** silent failures don’t kill the system.

Heartbeat is a distinct mechanism from cron; it can be configured to run regularly and follow HEARTBEAT.md strictly. ([OpenClaw][2])

**Practical heartbeat checks**

1. “Did Dwight’s intel update within 26 hours?”
2. “Did key cron jobs run?” (stale run detection)
3. “Is the browser/session dependency alive?” (if you use it)

Important nuance: OpenClaw heartbeat behavior is documented, but there may be version-specific quirks (there’s even a public issue about missing/empty HEARTBEAT.md behavior). So: keep HEARTBEAT.md present and non-empty to avoid accidental disabling. ([OpenClaw][2])

**Success criteria**

- If a cron job fails, heartbeat catches it and triggers recovery actions (or at least alerts you).

---

## 6) Operating procedures (how you run this day-to-day)

### Daily routine (10 minutes)

1. Open Telegram
2. Review Dwight intel
3. Review Kelly/Rachel drafts
4. Approve/reject with short feedback
5. Ensure feedback is logged (daily log + promote if repeated)

### Weekly routine (30–45 minutes)

1. Memory cleanup

   - archive old daily logs
   - dedupe contradictory rules
2. Promote repeated feedback

   - MEMORY.md
   - shared-context/FEEDBACK-LOG.md
3. Tune schedules if needed

### Biweekly routine (important)

Token bloat is real (daily logs explode).
Your rule should be:

- only load today + yesterday logs in startup
- archive older logs

---

## 7) Cron vs Heartbeat (clear practical rule)

Use **Cron** when:

- exact schedule matters (“8:01 AM daily”)
- you want isolated runs with predictable triggers
  Cron is the gateway scheduler and is designed for recurring tasks. ([OpenClaw][5])

Use **Heartbeat** when:

- you want health checks
- you want “catch failures + recover/alert”
  Heartbeat has a response contract (HEARTBEAT_OK behavior, ack trimming) that affects delivery—follow the doc. ([OpenClaw][2])

---

## 8) Practical safety model (do not skip)

Minimum safe practices:

- Separate keys for agents
- Least privilege
- Don’t store secrets in widely loaded files
- Avoid installing random extensions/skills; keep your system auditable (there have been real malware reports in add-on ecosystems). ([The Verge][4])

---

## 9) Rollout timeline (exactly what to do when)

### Week 1: Monica only

- identity + ops + daily log discipline
- 1 cron job
- 1 output

### Week 2: Dwight

- intel pipeline (md + json)
- Monica becomes coordinator, not researcher

### Week 3: Kelly + Rachel

- downstream drafting
- add style guides only if corrections repeat

### Week 4: Ross + Heartbeat

- engineering pipeline
- self-healing after first failure

---

## 10) Acceptance checklist (you are “done” when)

- ✅ You wake up to usable intel + drafts
- ✅ You spend <15 minutes reviewing
- ✅ Corrections stop repeating (because they’re written to memory)
- ✅ Jobs don’t silently die (heartbeat alerts/recovery)
- ✅ No coordination conflicts (one writer per shared file)

---

## Visual assets placement (from your images)

1. **Scheduling Table** — place in “Always-On Scheduling”
2. **Agent OS Layers** — place in “Three-layer Agent OS”
3. **Telegram Chat UX** — place in “Telegram as Control Plane”
4. **Work Example (Ross PR triage screenshot)** — place in “Engineering Agent Output Contract”

If you want, tell me your exact final agent list (6 or 8?) and your timezone, and I’ll tailor:

- the schedule ordering (to prevent downstream reading stale intel)
- the ownership matrix (who writes what)
- the “definition of done” templates per agent
  (all still **no code**, pure operational plan).

[1]: https://docs.openclaw.ai/concepts/agent-workspace?utm_source=chatgpt.com "Agent Workspace - OpenClaw"
[2]: https://docs.openclaw.ai/gateway/heartbeat?utm_source=chatgpt.com "Heartbeat - OpenClaw"
[3]: https://docs.openclaw.ai/channels/telegram?utm_source=chatgpt.com "Telegram - OpenClaw"
[4]: https://www.theverge.com/news/874011/openclaw-ai-skill-clawhub-extensions-security-nightmare?utm_source=chatgpt.com "OpenClaw's AI 'skill' extensions are a security nightmare"
[5]: https://docs.openclaw.ai/automation/cron-jobs?utm_source=chatgpt.com "Cron Jobs - OpenClaw"
