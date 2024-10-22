# NodeJS Specific

If you're diving into the world of JavaScript, you're probably using NodeJS for everything from backend services to front-end frameworks. It's everywhere, from Hardhat Framework to quick prototyping with scaffold-eth2. But with great power comes great responsibility, and NodeJS projects can have their fair share of security hiccups.

Let's talk about some of the common attack vectors you might encounter:

- **Dependency Confusion**: Imagine you're working on a private package, and suddenly, a malicious actor publishes a package with the same name to a public repository. If your setup isn't careful, you might end up pulling in the wrong package. Oops!

- **Outdated or Vulnerable Packages**: It's easy to forget to update your dependencies. But those old versions might have known vulnerabilities that can be exploited. It's like leaving your front door unlocked because you forgot to upgrade your lock.

- **Malicious Packages**: Sometimes, a package might look legit but has some nasty surprises inside. Always double-check what you're installing, or you might end up with more than you bargained for.

- **Misconfigurations**: A simple misconfiguration can open up your app to attacks. It's like leaving a window open when you go on vacation—inviting trouble.

Now, let's introduce some tools that can help you tackle these issues. They all somewhat behave similarly, comparing versions of a project packages to vulnerable packages, except for nodejsscan that can also check code:

## RetireJS

A [fast and simple scanner](https://github.com/RetireJS). It scans your code and dependencies, giving you a heads-up on what needs updating.

```js
wanderer@trg NodeGoat $ retire
retire.js v5.2.4
Downloading https://raw.githubusercontent.com/RetireJS/retire.js/master/repository/jsrepository-v4.json ...
...

/home/wanderer/NodeGoat/node_modules/nyc/node_modules/handlebars/dist/handlebars.amd.js
 ↳ handlebars 4.0.5
handlebars 4.0.5 has known vulnerabilities:

severity: high; summary: A prototype pollution vulnerability in handlebars is exploitable if an attacker can control the template, retid: 43; https://github.com/wycats/handlebars.js/commit/7372d4e9dffc9d70c09671aa28b9392a1577fd86 https://snyk.io/vuln/SNYK-JS-HANDLEBARS-173692

severity: high; summary: A prototype pollution vulnerability in handlebars is exploitable if an attacker can control the template, issue: 1495, githubID: GHSA-q42p-pg8m-cqh6; https://github.com/advisories/GHSA-q42p-pg8m-cqh6 https://github.com/wycats/handlebars.js/commit/cd38583216dce3252831916323202749431c773e https://github.com/wycats/handlebars.js/issues/1495 https://snyk.io/vuln/SNYK-JS-HANDLEBARS-174183

severity: high; summary: Disallow calling helperMissing and blockHelperMissing directly, retid: 44, CVE: CVE-2019-19919, githubID: GHSA-w457-6q6x-cgp9; https://github.com/wycats/handlebars.js/blob/master/release-notes.md#v430---september-24th-2019 

severity: high; summary: Regular Expression Denial of Service in Handlebars, githubID: GHSA-62gr-4qp9-h98f, CVE: CVE-2019-20922; https://nvd.nist.gov/vuln/detail/CVE-2019-20922

severity: medium; summary: Affected versions of `handlebars` are vulnerable to Denial of Service. The package's parser may be forced into an endless loop while processing specially-crafted templates. This may allow attackers to exhaust system resources leading to Denial of Service.

## Recommendation
Upgrade to version 4.4.5 or later., retid: 75, githubID: GHSA-f52g-6jhx-586p; https://github.com/handlebars-lang/handlebars.js/commit/f0589701698268578199be25285b2ebea1c1e427 severity: high; summary: Versions of `handlebars` prior to 3.0.8 or 4.5.2 are vulnerable to Arbitrary Code Execution. The package's lookup helper fails to properly validate templates, allowing attackers to submit templates that execute arbitrary JavaScript in the system. It can be used to run arbitrary code in a server processing Handlebars templates or on a victim's browser (effectively serving as Cross-Site Scripting).

The following template can be used to demonstrate the vulnerability:
    {{#with "constructor"}}
        {{#with split as |a|}}
            {{pop (push "alert('Vulnerable Handlebars JS');")}}
            {{#with (concat (lookup join (slice 0 1)))}}
                {{#each (slice 2 3)}}
                    {{#with (apply 0 a)}}
                        {{.}}
                    {{/with}}
                {{/each}}
            {{/with}}
        {{/with}}
    {{/with}}
```

Although we don't particularly like the output, at least for its CLI version, we have to admit it is incredibly fast. This is probably because it only looks through locked versions.

## Better NPM Audit

[Better NPM Audit](https://www.npmjs.com/package/better-npm-audit) takes the standard npm audit and kicks it up a notch. It provides more detailed insights and helps you prioritize which vulnerabilities to tackle first.

```bash
wanderer@trg NodeGoat $ better-npm-audit audit
╔═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════╗
║                                                                                       === npm audit security report ===                                                                                       ║
║                                                                                                                                                                                                               ║
║ ID      │ Module               │ Title                                              │ Paths                                              │ Severity │ URL                                               │ Ex. ║
║ 1093814 │ adm-zip              │ Arbitrary File Write in adm-zip                    │ adm-zip                                            │ moderate │ https://github.com/advisories/GHSA-3v6h-hqm4-2rg6 │ n   ║
║ 1097685 │ ajv                  │ Prototype Pollution in Ajv                         │ ajv                                                │ moderate │ https://github.com/advisories/GHSA-v88g-cgmw-v5xw │ n   ║
║ 1094090 │ ansi-regex           │ Inefficient Regular Expression Complexity in       │ ansi-align>ansi-regex                              │ high     │ https://github.com/advisories/GHSA-93q8-gq69-wqmw │ n   ║
║         │                      │ chalk/ansi-regex                                   │ boxen>ansi-regex                                   │          │                                                   │     ║
║         │                      │                                                    │ widest-line>ansi-regex                             │          │                                                   │     ║
║ 1097691 │ async                │ Prototype Pollution in async                       │ async                                              │ high     │ https://github.com/advisories/GHSA-fwr7-v2mv-hh25 │ n   ║
║         │                      │                                                    │ grunt-retire>form-data>async                       │          │                                                   │     ║
║ 1096879 │ babel-traverse       │ Babel vulnerable to arbitrary code execution when  │ nyc>babel-traverse                                 │ critical │ https://github.com/advisories/GHSA-67hx-6x53-jw92 │ n   ║  
╚═════════╧══════════════════════╧════════════════════════════════════════════════════╧════════════════════════════════════════════════════╧══════════╧═══════════════════════════════════════════════════╧═════╝

136 vulnerabilities found. Node security advisories: 1093814, 1097685, 1094090 ...
```

## NodeJsScan

Don't expect [this one](https://github.com/ajinabraham/NodeJsScan) to run as fast as the previous ones, a least not for huge codebases, since it checks all installed dependencies as well. It is a static analyzer that checks your code for common security issues and vulnerabilities, whether it's a misconfiguration or a sneaky piece of code.

```bash
wanderer@trg ~ $ nodejsscan -d NodeGoat -o report.json

[INFO] Running Static Analyzer on - NodeGoat
{
    "files": [
        {
            "/allocations.js": "app/routes/allocations.js"
        },
        {
            "/research.js": "app/routes/research.js"
        }
        ...
    ],
    ...
    "sec_issues": {
        "Open Redirect Vulnerability": [
            {
                "description": "Untrusted user input in redirect() can result in Open Redirect vulnerability",
                "filename": "index.js",
                "line": 72,
                "lines": "    app.post(\"/memos\", isLoggedIn, memosHandler.addMemos);\n\n    // Handle redirect for learning resources link\n    app.get(\"/learn\", isLoggedIn, (req, res) => {\n        // Insecure way to handle redirects by taking redirect url from query string\n        return res.redirect(req.query.url);\n    });\n\n    // Research Page\n    app.get(\"/research\", isLoggedIn, researchHandler.displayResearch);",
                "path": "app/routes/index.js",
                "sha2": "70a4402d7593c404e815aa800f53aa870b09e1b2c940038463c05bacf43def00",
                "tag": "opr",
                "title": "Open Redirect"
            }
        ],
        "Remote Code Injection": [
            {
                "description": "User controlled data in eval() can result in Server Side Injection (SSI) or Remote Code Execution (RCE).",
                "filename": "contributions.js",
                "line": 32,
                "lines": "this.handleContributionsUpdate = (req, res, next) => {\n\n        /*jslint evil: true */\n        // Insecure use of eval() to parse inputs\n        const preTax = eval(req.body.preTax);\n        const afterTax = eval(req.body.afterTax);\n        const roth = eval(req.body.roth);\n\n        /*",
                "path": "app/routes/contributions.js",
                "sha2": "0653948d7144fc9a768e555749cf9ebba4e20d47d5e3466cd4e0cf08d53eb608",
                "tag": "rci",
                "title": "Server Side Injection(SSI) - eval()"
            },
            {
    "total_count": {
        "good": 11,
        "mis": 9,
        "sec": 4
    },
    "vuln_count": {
        "Open Redirect": 1,
        "Server Side Injection(SSI) - eval()": 3
    }
```

The container version has a web ui, where you can throw repositories into it or clone them directly, and also provides a graphical view of the report. I'd suggest using the cli with a json report unless you need to show the findings summary to an executive.

If you liked this tool, by the same author you can also try [njsscan](https://github.com/ajinabraham/njsscan#build-locally), which is a semantic aware SAST tool that can find insecure code patterns in your Node.js applications.

It uses a simple pattern matcher from **libsast** and syntax-aware semantic code pattern search tool **semgrep**.

```bash
wanderer@trg ~ $ njsscan NodeGoat
- Pattern Match ████████████████████████████████████████████████████████████ 87
- Semantic Grep  10

njsscan: v0.3.7 | Ajin Abraham | opensecurity.in
...
╒═════════════╤═══════════════════════════════════════════════════════════════════════════════╕
│ RULE ID     │ express_open_redirect                                                         │
├─────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ CWE         │ CWE-601: URL Redirection to Untrusted Site ('Open Redirect')                  │
├─────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ OWASP-WEB   │ A1: Injection                                                                 │
├─────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ DESCRIPTION │ Untrusted user input in redirect() can result in Open Redirect vulnerability. │
├─────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ SEVERITY    │ ERROR                                                                         │
├─────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ FILES       │ ╒════════════════╤═════════════════════════════════════╕                      │
│             │ │ File           │ NodeGoat/app/routes/index.js        │                      │
│             │ ├────────────────┼─────────────────────────────────────┤                      │
│             │ │ Match Position │ 16 - 43                             │                      │
│             │ ├────────────────┼─────────────────────────────────────┤                      │
│             │ │ Line Number(s) │ 72                                  │                      │
│             │ ├────────────────┼─────────────────────────────────────┤                      │
│             │ │ Match String   │ return res.redirect(req.query.url); │                      │
│             │ ╘════════════════╧═════════════════════════════════════╛                      │
╘═════════════╧═══════════════════════════════════════════════════════════════════════════════╛
```
