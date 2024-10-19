# Fake Analyzer

This one allows you to select a username, without the need for a repository or an organization. It just looks through almost anything it can find and creates a JSON report for you to inspect.

Let's start with an example of their repo, running the tool against `eduales99`.

```bash
gh-analyze --token github_pat_11AABCCDDEE13802849209HD09283CDFFF eduales99
```

It begins displaying the requests it is doing, like followers/following, info about their repos, and more, you can check the rest of the log in `script.log`. The report will be found at `out/eduales99/report.json`. You can specify where to ouput the files if you wanted to.

Let's see some of the fields it outputs.

Emails associated with that account from interactions:

```json
    "unique_emails": [
        {
            "email": "eduales9829@gmail.com",
            "name": "eduales99"
        },
        {
            "email": "finaldev.dream119@gmail.com",
            "name": "Never Give Up"
        },
        {
            "email": "noreply@github.com",
            "name": "GitHub"
        },
        {
            "email": "49699333+dependabot[bot]@users.noreply.github.com",
            "name": "dependabot[bot]"
        },
        {
            "email": "121861323+ngud-0119@users.noreply.github.com",
            "name": "NGUD-0119: ACE"
        },
        {
            "email": "finaldev.dream.119@gmail.com",
            "name": "NGUD-0119"
        },
        {
            "email": "156386147+eduales99@users.noreply.github.com",
            "name": "Eduardo Morales Cortes"
        },
        {
            "email": "ortegamontenegrosebastian@gmail.com",
            "name": "sebastian4098"
        }
    ],
```

Repository list, as you can see a lot of web3 projects in there..

```json
"repo_list": [
    "auction-dApp-Ethereum",
    "bancor-protocol-contracts",
    "boilerplate-ethereum",
    "borrowAndrepayNFT",
    "cryptowallet-app",
    "d3-chart",
    "Developer-Dashboard",
    "dfs-token-contracts",
    "eduales99",
    "hardhart-smart-contract",
    "Javascript-Drawio",
    "lottery-contract",
    "TensorFlow-Models",
    "Viking-P2E-Game"
],
```

Even better yet, it shows you direct contributions to their repos:

```json
"contributors": [
    {
        "repo": "auction-dApp-Ethereum",
        "contributors": [
            "eduales99"
        ]
    },
    {
        "repo": "lottery-contract",
        "contributors": [
            "sebastian4098",
            "eduales99"
        ]
    },
```

Contributions to other repositories.

```json
"commits_to_other_repos": [
    {
        "repo": "sebastian4098/portfolio-GASP",
        "commits": [
            "2cf64d2ccb957eb37e6ee86d886f5ba19f268196",
            "a9e0971b14138fbe7baa467256072fd56afbefca"
        ]
    },
    {
        "repo": "sebastian4098/express-server",
        "commits": [
            "97c305583102afb9d904e9dd7b8f941dd926d3ca"
        ]
    }
],
```

From this, we can conclude there might be a relationship between `sebastian4098` and `eduales99`.

Another interesting feature, is the "potential copy" section where it checks if the account was created after commit messages (no fork).

```json
"potential_copy": [
    {
        "repo": "auction-dApp-Ethereum",
        "reason": "account creation date later than the first commit to the repository",
        "commit_date": "2017-07-01T00:13:16-05:00"
    },
    {
        "repo": "bancor-protocol-contracts",
        "reason": "account creation date later than the first commit to the repository",
        "commit_date": "2020-08-07T13:57:17+03:00"
    },
    {
        "repo": "boilerplate-ethereum",
        "reason": "account creation date later than the first commit to the repository",
        "commit_date": "2021-10-31T12:02:11+02:00"
    },
    {
        "repo": "borrowAndrepayNFT",
        "reason": "account creation date later than the first commit to the repository",
        "commit_date": "2022-09-17T23:49:03+09:00"
    },
    ...
]
```

There's much more we can do, and in case something gets deleted or tampered with, we have at least some of that data as a back-up.

Another cool feature it has, is that you can also monitor an account with `gh-monitor`.

```json
wanderer@trg eduales99 $ gh-monitor -u mattaereal # removing token because of space
Starting to monitor activity for users: mattaereal
Press Ctrl+C to stop monitoring.
User: mattaereal, mattaereal starred the repository aquasecurity/cloudsploit, Date: 2024-10-17T19:56:14Z
User: mattaereal, mattaereal created a branch in theredguild/DevSecOps-toolkit, Date: 2024-10-17T19:48:27Z
User: mattaereal, mattaereal opened an issue in security-alliance/frameworks, Date: 2024-10-17T19:27:32Z
User: mattaereal, mattaereal updated the wiki in theredguild/DevSecOps-toolkit, Date: 2024-10-17T17:46:13Z
....
User: mattaereal is now following theredguild
User: mattaereal changed their name from 'None' to 'Matías Aereal Aeón'
User: mattaereal changed their company from 'None' to 'The Red Guild @theredguild '
User: mattaereal changed their blog from 'None' to 'blog.theredguild.org'
User: mattaereal changed their bio from 'None' to 'Hacker. Security generalist.
Doing quests @theredguild'
User: mattaereal changed their twitter_username from 'None' to 'mattaereal'
User: mattaereal profile was updated at 2024-08-01T14:05:00Z
```

If you're suspicious and want to track the behavior of a specific account, you can check it out. Some cybercriminals change usernames, delete repos, trying to start anew.
