# Use Thinking Machine Method

A convention for using AI agents as a thinking partner — structured brainstorming through one-by-one questioning, with a living document that stays coherent throughout the conversation.

## Why

- **One question at a time steers correctly.** When the agent asks multiple questions at once, the follow-up questions are based on assumptions about how the user would answer the earlier ones. Those assumptions may be wrong or hallucinated. One at a time forces the agent to listen before asking the next question.
- **The living document survives context rot.** AI agents lose coherence past roughly 40% of their context window. Writing everything down at the end of a long conversation produces a document full of gaps. Rewriting with every answer keeps the truth in the file, not in fading memory.
- **Subagents preserve the primary context.** Offloading the rewrite to a subagent keeps the main conversation lean and focused on thinking, not file editing.

## Protocol

When the user asks you to brainstorm, think through, or explore an idea using this method, follow this protocol exactly.

### 1. Create BRAINSTORM.md

Create the file with a brief summary of the user's idea as described in their initial message. Keep it short — one or two paragraphs.

### 2. Ask one question

Ask the user **one question** to clarify, explore, or extend their idea. Only one. Wait for their answer before asking the next.

With each question, briefly provide your own lightweight analysis — relevant trade-offs, industry standards, or alternative approaches — to help surface things the user may not have considered. Keep it brief and informative, not overwhelming.

### 3. Rewrite BRAINSTORM.md

After the user answers, spawn a subagent to:

1. Reread BRAINSTORM.md in full.
2. Rewrite it entirely to reflect the user's answer naturally, as if the answer was always part of the original idea.

The subagent must not append, patch, or add references to the conversation. The file should always read as a clean, standalone, original document.

### 4. Repeat

Go back to step 2. Continue until:

- The topic has been fully explored and there are no meaningful questions left to ask, or
- The user indicates they want to stop.

## Rules

- **Never ask more than one question per message.** This is the most important rule.
- **Never append to BRAINSTORM.md.** Always reread and rewrite the entire file.
- **Always use a subagent for the rewrite.** The primary conversation context is for thinking, not file operations.
- **Never reference the conversation in the file.** Lines like "as previously mentioned" or "updated from earlier" must never appear. The file is not a changelog.
- **End naturally.** Do not force a fixed number of questions. Stop when the idea is complete or the user says so.
