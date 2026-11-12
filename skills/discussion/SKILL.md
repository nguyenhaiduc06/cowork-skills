---
name: discussion
description: Run a multi-person design discussion on GitHub Issues, from discussion issue to signed-off shared spec. Use when the user wants to open, propose points for, sync, request sign off on, or close a discussion, or amend a shared spec.
---

A discussion is between several **participants**, each owning one **scope**. One of them is the **facilitator** (named in the issue body): they drive the discussion and own the body. The others are **contributors**: they comment, by hand or with Claude, so everything must work for someone who only reads the issue.

First work out the user's **role**: the facilitator is whoever runs `open`; on an existing issue, compare `gh api user --jq .login` with the Facilitator line in the body.

- **Facilitator**: every step below is theirs.
- **Contributor**: help them read the discussion (summarise comments and Open points), draft their comments and replies to `P` points, and review drafts. Leave the issue body, `propose`, `signoff`, `close` and `amend` to the facilitator; if the contributor wants one of those, draft a comment asking the facilitator for it.

Lifecycle: **discussion** issue → **shared spec** issue → one or more **scoped spec** issues per participant (sub-issues of the shared spec) → tickets. This skill covers the discussion and the shared spec only. Scoped specs and tickets belong to each participant's own workflow.

GitHub Issues through `gh` only. Write issues and comments in the language the user is speaking to you, and translate the template headings to match.

## Ground rules

- **The body is the summary, the comments are the conversation.** You keep the issue body up to date; the discussion itself happens in comments.
- **Voice**: every comment you post starts with `🤖 Claude (for @<user>):`, the user being whoever you're working for. Show them the draft comment and post it only after they approve. Body edits (facilitator only) need no approval.
- **Sign off** is a comment `✅ sign off` written by the participant themselves. You never post a sign off for anyone, the facilitator included. A sign off counts only for the latest draft posted before it; a new draft resets the checklist.
- **The facilitator decides.** A point moves to Decided only when the facilitator confirms it, even if every contributor agreed.
- **Shared spec = boundaries only**: the problem, the user-facing solution, who owns which scope, the contract between participants (with sample payloads and error cases), out of scope. Decisions internal to one scope stay out, and so do contracts internal to one participant.
- Labels: the discussion issue carries `discussion`, the shared spec carries `spec`, and neither carries a triage label. Before creating either issue, make sure the label exists: `gh label create discussion --color 5319E7 --description "Multi-person design discussion" --force` (and `spec`, `0E8A16`, `"Shared spec agreed in a discussion"`).

## Steps

The user names a step in plain words ("open a discussion with @viet about…", "sync #80"). Run that step.

### open

1. Get the goal, and every participant as `@handle: scope`. The facilitator is a participant too.
2. Create the issue with label `discussion` and the body from [templates/discussion.md](templates/discussion.md). Facilitator = the `gh` user (`gh api user --jq .login`).
3. Done when the issue exists and you've given the facilitator its URL.

### propose

1. Draft the points that need a decision, numbered `P1`, `P2`…, each with options and your recommended answer. Write each so a contributor can reply `P1: ok` or `P1: no, because …`.
2. Facilitator approves, then post as one comment and @mention the participants each point concerns. Add the points to Open in the body.

### sync

1. Read **all** comments, every time: `gh api repos/{owner}/{repo}/issues/<n>/comments --paginate`.
2. Tell the facilitator what changed: new opinions, answers to `P` points, points that look agreed, new questions.
3. Facilitator confirms which points move to Decided; update the body's Decided, Open and Sign off sections. Sign off checklist: tick a participant only if they commented `✅ sign off` after the latest draft.
4. Offer a reply comment (your view, questions for contributors) if it would move things forward; post only after approval.

Run on a shared spec (`sync <spec #>`), sync instead refreshes its Scoped specs checklist from `gh api repos/{owner}/{repo}/issues/<n>/sub_issues`.

### signoff

1. Write the draft shared spec from Decided, using [templates/shared-spec.md](templates/shared-spec.md), headed `Draft v<N>` (N = previous draft + 1).
2. Facilitator approves, then post it as a comment that @mentions every participant and asks them to review and comment `✅ sign off` or what to change.
3. Reset the body's Sign off checklist to `Draft v<N>` with every participant unticked, linking the draft comment.
4. Re-running signoff with nothing changed posts a short reminder to those not yet signed off, with no new version.

### close

1. Only the facilitator closes. Check every participant signed off the latest draft. If anyone is missing, stop and name them: the way forward is to wait, or to remove that participant (below).
2. Confirm with the facilitator, then create the shared spec issue: label `spec`, body = the signed-off draft, with Discussion linked and a first Change log entry.
3. Comment the shared spec link on the discussion, then close the discussion. Closing by hand on GitHub is just as valid.

### amend

For a change to a shared spec that already exists. Contributors ask for a change by commenting on the shared spec.

1. Classify the change. **Minor** = additive and backward compatible (e.g. a new optional field). **Breaking** = removes or changes something, or moves a boundary between scopes. Propose the class; the facilitator decides.
2. Breaking: stop, and suggest opening a new discussion that links the shared spec.
3. Minor: post the change as a comment (`Amendment v<N>`) that @mentions the participants it affects and asks them to sign off. Once they all have, under the same sign off rules, edit the body and add a Change log entry: what changed, who signed off, link to the comment.

### Participants mid-way

When the facilitator adds or removes a participant, edit the body. A new participant is @mentioned with a pointer to "How to take part" and added unticked to the current draft's checklist. A removal is noted with its reason under Participants.
