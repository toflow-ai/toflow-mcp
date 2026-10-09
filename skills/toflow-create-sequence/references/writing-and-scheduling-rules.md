# Message Writing Cheat Sheet

Message-content quality and cadence judgment only. For exact node type names, scheduling fields, sending accounts, and edges, call `get_sequence_creation_guide` / `get_sequence_schema` directly — those are the live source of truth, not this file.

## Template variables — what to put in messages

- Safe to use freely: person's first/last name, job title.
- Always available: the sending workspace member's own name.
- Ask before using: company name/website — leads without linked company data may fail personalization.
- Never invent a variable value — resolve it from real person/company data or ask the user.
- Full, current list: `get_sequence_schema`.

## Email content

- Plain text vs HTML — always ask first. Plain text tends to have better deliverability for cold outreach; separate paragraphs with a blank line, never a single line break (most email clients hard-wrap short lines). HTML gives better formatting control. Pass only the field the chosen format uses, never both.
- Subject: 2-6 words, lowercase, question format or specific insight — never generic.
- Body: one value proposition, no fluff openers ("Hope this finds you well", "My name is...", "I work at..."), open with the point, end with a low-friction yes/no CTA.
- No em dashes. No sign-off/signature — appended server-side.

## LinkedIn / WhatsApp content

- Open with a specific observation (their post, role change, company milestone) — not a pitch.
- Open loop, don't reveal everything; ask one easy question, never a meeting ask in message one.
- No superlatives ("best-in-class", "game-changing"), no pressure language ("act now", "limited time").
- `send_linkedin_connection`: omit the message unless the user explicitly requests one. LinkedIn allows roughly 20-25 requests/day without a message, dropping to 8-10/day with one attached.
- `linkedin_inmail`: requires a premium LinkedIn account (Sales Navigator, Recruiter, or Premium Business) and consumes one InMail credit per send. Subject is required and must not be empty. Keep the message under ~400 characters (response rate drops noticeably above that) and send Monday-Thursday only. Never use it on someone already a 1st-degree connection — use `linkedin_message` instead.

## Gaps between steps

The schema enforces only that the wait is a positive number — it gives no cadence judgment. Propose these as defaults and confirm with the user, don't apply them silently:

| Between | Suggested gap | Why |
|---|---|---|
| Trigger → first outreach step | Same day, or 0-1 day | No reason to delay the opening touch once someone's enrolled |
| Email → next email (follow-up) | 2-4 days | Enough time to notice/respond without going cold; standard cold-email cadence |
| Connection request → whatever follows its branch | 3-5 days | Window for the recipient to accept before the branch resolves — too short and you're judging "not accepted" prematurely; too long and the sequence stalls |
| Profile view → next step | Same day to 1 day | Low-friction warm-up touch, not a message — short gap is fine |
| Direct message → next direct message | 3-5 days | Similar cadence to email follow-ups; avoid stacking messages faster than a real person would |
| Premium/InMail-style message → next step | 4-7 days | Often credit-limited monthly and a heavier ask than a DM — no reason to rush the next touch |
| WhatsApp message → next WhatsApp message | 1-2 days | Faster, more immediate channel than email/LinkedIn — shorter gaps read as normal there, not pushy |
| Last outreach step → sequence end | N/A | No further gap needed once the final step has run |

These are starting points, not fixed rules — always ask if the user has a preferred cadence before applying defaults, and adjust for context (e.g. a highly targeted ABM sequence may want longer gaps than a high-volume one).

## Sequence structure patterns by channel scope

Starting points to propose, not fixed templates — always confirm the shape and exact available node types (via `get_sequence_schema`) with the user before building. Branching is only available on `send_linkedin_connection` and `is_in_linkedin_network` (`true`/`false` edges, branches never reconverge).

**LinkedIn-only sequence:**
```
trigger → view_linkedin_profile (optional warm-up touch)
        → is_in_linkedin_network
             true  → linkedin_message (already connected, message directly)
             false → send_linkedin_connection (at most one node in this branch)
                        true  (accepted)     → linkedin_message
                        false (not accepted) → linkedin_inmail (needs premium account)
                                                or end here if no premium account
```
Check `is_in_linkedin_network` first rather than assuming not-connected — skips a redundant connection request to someone already in-network.

**Email-only sequence** — no conditional nodes needed, just linear follow-ups:
```
trigger → email (1st touch) → email (follow-up) → email (final/break-up)
```
Typically 2-4 emails total; more than that tends to fatigue rather than convert.

**Multi-channel sequence** — email as primary, LinkedIn as a parallel or fallback touch:
```
trigger → email (1st touch)
        → view_linkedin_profile or send_linkedin_connection (LinkedIn touch alongside/after)
             true/false branch as in the LinkedIn-only pattern above
        → email (follow-up, regardless of LinkedIn branch outcome)
        → whatsapp_message (if a phone number is available and the user wants it as a channel)
```
Don't default to using every channel just because the person has data for it — ask the user which channels they actually want in this sequence before drafting nodes for all of them.

## AI-personalized nodes (`ai_prompt`)

- Any outreach-step node can carry an `ai_prompt` instead of manual content.
- Content is generated per-person at enrollment time — leave manual content fields empty on that node.
- Never draft content yourself for these nodes, at creation or at enrollment.

## Avoid AI writing tells

Applies to every piece of content you draft for a node — subjects, bodies, LinkedIn/WhatsApp messages — at both sequence-creation and enrollment-personalization time. A prospect who smells AI-written outreach disengages immediately, so this isn't a style preference, it's a deliverability concern.

**Punctuation:**
- No em dashes (—), anywhere, ever. Use a comma, a period, or rewrite the sentence.
- Don't use a hyphen as a stand-in dash either (` - ` mid-sentence to set off a clause).
- Don't over-format with bullets/bold inside a message — these are chat/email messages to one person, not a landing page. Plain sentences.

**Banned words and phrases** (instant AI tell in cold outreach):
- "I hope this email finds you well", "I wanted to reach out", "In today's fast-paced world"
- "Delve into", "leverage" (as a verb), "seamless", "cutting-edge", "game-changer", "robust", "unlock", "streamline", "empower"
- "This means that...", "This allows you to...", "This ensures that..."
- "Not just X, but Y" constructions
- "At the end of the day", "the reality is", "the truth is"
- Any superlative stack ("best-in-class", "revolutionary", "world-class")

**Structure tells to avoid:**
- Don't open with a scene-setting or throat-clearing sentence before the actual point — specifically watch for AI's favorite move of restating the recipient's situation back to them as a fake-personalized opener.
- Don't write in the three-sentence paragraph formula (bold claim → supporting sentence → wrap-up sentence) — it reads as templated even when the content is personalized.
- Vary sentence length. A message where every sentence is 12-18 words reads as machine-generated regardless of content quality.
- Don't end with a stacked CTA ("Would you be open to a quick call, or happy to share more info if useful?") — one clear ask.

**Before finalizing any drafted content:** read it back and ask whether it sounds like a person who knows this prospect wrote it in two minutes, or like a template with variables filled in. If it's the latter, rewrite — don't just swap a word or two.
