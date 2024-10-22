# GitHub: Tindersec's Workflow Auditor

And start by using a tool called [**`gh-workflow-auditor`**](https://github.com/tindersec/gh-workflow-auditor) by @TinderSec. This tool not only scans for actions, but the entire workflow. It looks for malicious or ill-intended inputs, from commits' title messages, to author's name or email fields.

Exporting PAT on the same line, run it like this:

```bash
wanderer@trg ~ $ PAT=github_pat_11ahshdasadasdkjasdhksajdsaj gh-workflow-auditor --type repo step-security-github-actions-goat
```

Since we aren't using a real setup (a repository with commits, issues), and in this case it is only looking through a bunch of yml files, the full potential of the tool is not being exploited.
