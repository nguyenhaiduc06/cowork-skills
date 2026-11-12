# cowork-skills

Skills for running design discussions between several people on GitHub Issues. One person, the facilitator, drives the discussion; everyone else contributes by commenting. Anyone can use the skill in either role, and contributors don't need Claude at all.

Only the discussion part is covered. Turning a shared spec into each person's own specs and tickets stays with whatever workflow each person uses.

## Install

```sh
npx skills add nguyenhaiduc06/cowork-skills
```

Needs [`gh`](https://cli.github.com/), logged in to the repo where discussions happen.

## Lifecycle

```
discussion ──(everyone signs off)──▶ shared spec ──▶ scoped spec (≥1 per participant) ──▶ tickets
```

| Step      | What happens                                                                  |
| --------- | ----------------------------------------------------------------------------- |
| `open`    | Facilitator opens a `discussion` issue and lists participants with scopes.    |
| `propose` | Claude drafts numbered points to decide (`P1`, `P2`…); contributors reply.    |
| `sync`    | Claude reads all comments and updates Decided / Open in the issue body.       |
| `signoff` | Claude posts a versioned draft shared spec + contract; everyone signs off.    |
| `close`   | Once all signed off: create the `spec` issue, link it, close the discussion.  |
| `amend`   | Minor change to a shared spec: affected people sign off again, log it.        |

Say the step in plain words to Claude: "open a discussion with @viet about fault detection", "sync #80", "close #80".

## Rules everyone follows

- The issue body is a summary kept by the facilitator; the conversation is in comments.
- Sign off = comment `✅ sign off` yourself. It counts for the latest draft only. Claude never signs off for anyone.
- Comments starting with `🤖 Claude (for @x):` were written by Claude and approved by @x.
- The shared spec holds boundaries only: problem, solution, scopes, the contract between participants, out of scope.
- Each participant attaches at least one scoped spec as a sub-issue of the shared spec. Whoever closes the last one closes the shared spec by hand.
- Minor spec change (additive) → comment on the spec. Breaking change or new boundary → new discussion.

## Jira mapping

| Here           | Roughly in Jira                     |
| -------------- | ----------------------------------- |
| discussion     | refinement meeting                  |
| shared spec    | epic                                |
| scoped spec    | child epic / per-team feature       |
| ticket         | story / task                        |

## Example

Feature "faults to fix + drills", two participants (FE + workers, Analysis):

1. `open` → discussion #80.
2. The Analysis owner comments by hand; the facilitator runs `sync`.
3. `signoff` → Draft v1 with contract `result.json`: adds `shots[].faults[]` (`code` one of 5 values, `confidence`, `frame_index`).
4. Both comment `✅ sign off`.
5. `close` → shared spec #81; #80 closed.
6. Each side writes a scoped spec as a sub-issue of #81 (#82, #87), then their own tickets.
7. Analysis needs an extra field `side` → `amend`, minor, both sign off again on #81. #82 and #87 close, then #81.

## Tiếng Việt (cho người tham gia)

Bạn không cần dùng Claude. Cứ comment trên issue `discussion`. Với các điểm `P1`, `P2`…, trả lời theo số, ví dụ `P1: ok` hoặc `P2: không, vì …`. Khi có bản nháp spec, đọc rồi comment `✅ sign off`, hoặc ghi chỗ cần sửa. Sign off chỉ tính cho bản nháp mới nhất. Sau khi có shared spec, tạo scoped spec của bạn rồi gắn nó làm sub-issue của shared spec. Hướng dẫn gắn có sẵn trong body của shared spec.

## License

MIT
