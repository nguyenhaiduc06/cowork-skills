Discussion: #<n>

## Problem

<from the user's point of view>

## Solution

<from the user's point of view>

## Scopes

- @<handle>: <scope>: <what it owns at the boundary>

## Contract

<!-- only what crosses between participants -->

### <contract name, e.g. result.json>

Producer: @<handle> → consumer: @<handle>

```json
<sample payload>
```

Errors:

- <case>: <what the producer sends, what the consumer does>

## Out of scope

-

## Scoped specs

Every participant attaches at least one scoped spec as a sub-issue of this issue.

- [ ] @<handle>: <scope>: _none yet_

To attach one: on GitHub, open this issue → Sub-issues → "Add existing issue" or "Create sub-issue". Or from the CLI:

```sh
child_id=$(gh api repos/<owner>/<repo>/issues/<scoped spec #> --jq .id)
gh api repos/<owner>/<repo>/issues/<this #>/sub_issues -X POST -F sub_issue_id=$child_id
```

GitHub does not close this issue on its own: whoever closes the last scoped spec closes this one by hand.

## Changing this spec

- Minor (additive, backward compatible, e.g. a new optional field): comment here. The participants it affects sign off again, then the body and Change log are updated.
- Breaking, or a change of boundaries: open a new discussion.

## Change log

- v1: agreed in #<discussion>; signed off by @<handle>, @<handle>
