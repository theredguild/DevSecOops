# Semgrep

We are going to start with [**`Semgrep`**](https://semgrep.dev/), which is a powerful, customizable lightweight static analysis tool for many languages. Out of the box works like a charm, but if you login, which is free, you have access to the rest of the rules.

```bash
wanderer@trg ~ $ semgrep login
Login enables additional proprietary Semgrep Registry rules and running custom policies from Semgrep Cloud Platform.
Opening login at: https://semgrep.dev/login?cli-token=abc12345-6789-0123-4567-89abcdef0123&docker=False&gha=False

Once you've logged in, return here and you'll be ready to start using new Semgrep rules.
Saved login token

	3b9f1a7c8d4e2f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0

in /home/wanderer/.semgrep/settings.yml.
Note: You can always generate more tokens at https://semgrep.dev/orgs/-/settings/tokens
```

If you use `semgrep ci`, by default it will scan much more than just secrets. Not to deviate from the main topic, we're going to restrict it to what we want. So we will use two generated rules, one by semgrep which is secrets and has many pro rules, and the other a community ruleset based on a popular tool named `gitleaks`. There are many tools that have been ported to semgrep, you'll see!

```bash
wanderer@trg ~ $ semgrep --config "p/secrets" fake-leaks

┌──── ○○○ ────┐
│ Semgrep CLI │
└─────────────┘

METRICS: Using configs from the Registry (like --config=p/ci) reports pseudonymous rule metrics to semgrep.dev.
To disable Registry rule metrics, use "--metrics=off".
Using configs only from local files (like --config=xyz.yml) does not enable metrics.

More information: https://semgrep.dev/docs/metrics


Scanning 205 files (only git-tracked) with 268 Code rules:

  CODE RULES

  Language      Rules   Files          Origin      Rules
 ─────────────────────────────        ───────────────────
  <multilang>      37     196          Pro rules     216
  yaml              1      27          Community      52
  python           58       4
  php               1       2
  js               36       1
  go               17       1

  SUPPLY CHAIN RULES

  💎 Run `semgrep ci` to find dependency
     vulnerabilities and advanced cross-file findings.


  PROGRESS

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╸  99% 0:00:00


┌───────────────────┐
│ 103 Code Findings │
└───────────────────┘
...
fake-leaks/semgrep-rules-examples/detected-google-oauth-access-token.txt
❯❯❱ generic.secrets.security.detected-google-oauth-access-token.detected-google-oauth-access-token
        Google OAuth Access Token detected
        Details: https://sg.run/ox2n

        3┆ "access_token" : "ya29.AHES67zeEn-RDg9CA5gGKMLKuG4uVB7W4O4WjNr-NBfY6Dtad4vbIZ",
...
(continues)
...
┌──────────────┐
│ Scan Summary │
└──────────────┘
Some files were skipped or only partially analyzed.
  Scan was limited to files tracked by git.
  Partially scanned: 6 files only partially analyzed due to parsing or internal Semgrep errors
  Scan skipped: 9 files matching .semgrepignore patterns
  For a full list of skipped files, run semgrep with the --verbose flag.

Ran 150 rules on 196 files: 103 findings.

📢 Too many findings? Try Semgrep Pro for more powerful queries and less noise.
   See https://sg.run/false-positives.
```

Now let's give it a try to `gitleaks` port, only showing the summary not to create 

```bash
semgrep --config "p/gitleaks"
┌──────────────┐
│ Scan Summary │
└──────────────┘
Some files were skipped or only partially analyzed.
  Scan was limited to files tracked by git.
  Scan skipped: 9 files matching .semgrepignore patterns
  For a full list of skipped files, run semgrep with the --verbose flag.

Ran 175 rules on 196 files: 523 findings.

📢 Too many findings? Try Semgrep Pro for more powerful queries and less noise.
   See https://sg.run/false-positives.

```

We will be using semgrep in other sections as well, since it can parse anything you set your mind to.
