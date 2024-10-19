# DevSecOops

![AI Generated](oops.png)

Hi! And welcome to the DevSecOops repository. An educational repository complementary to our [@theredguild/devsecops-toolkit](https://github.com/theredguild/devsecops-toolkit) where we provide practical aid on how to use some of the tools depicted there.

To start, you should have at least built the `workshop-minimal` image of that container.

```bash
git clone https://github.com/theredguild/devsecops-toolkit
git checkout workshop-minimal
make exec
```

You should be able to see something similar to this.

```plaintext
❯ make exec
fatal: No names found, cannot describe anything.
Running interactive shell inside the devsecops-toolkit container...
                    __        __   _                               
                    \ \      / /__| | ___ ___  _ __ ___   ___      
                     \ \ /\ / / _ \ |/ __/ _ \| '_ ` _ \ / _ \     
                      \ V  V /  __/ | (_| (_) | | | | | |  __/     
                    __ \_/\_/ \___|_|\___\___/|_| |_| |_|\___|     
                    \ \      / /_ _ _ __   __| | ___ _ __ ___ _ __ 
                     \ \ /\ / / _` | '_ \ / _` |/ _ \ '__/ _ \ '__|
                      \ V  V / (_| | | | | (_| |  __/ | |  __/ |   
                       \_/\_/ \__,_|_| |_|\__,_|\___|_|  \___|_|   

                         Welcome to the devsecops toolkit
                            Built by The Red Guild 🪷

    This container was created as a resource for a workshop, 
    which intends to spread awareness, help people protect themselves 
    and the repos they interact with. Say hi @theredguild!, don't be a stranger.

wanderer@trg ~ $ 
```

We don't use these tools daily, but they are a selection and a curation of our own after thorough research, testing, and asking around :). If you want more tools, you can checkout `develop` but today in its current state it weights like +10GB.

## GitHub analysis

As you already know, there has been a surge in fake profiles and impersonation in github profiles, contributors, or even backdoored repositories. So here we bring you two tools to help you get some of the basic information you need in order to be able to identify these cases.

### GitXRay

One tool that works out of the box is `gitxray`, by a fellow hacker from the @ekoparty, Lucas.
The only thing I'd suggest doing if you want to actually do a profound analysis, is that by default it uses GH's public access token, so you'll need to add an environment variable when you call it. Othwerwise, to do a shallow an quick analysis you're fine.

In case you want to do it, I'll guest creating a Fine-grained Personal Access Token that expires soon enough, with a representative name and Read Only access to Public Repositories.

```bash
GH_ACCESS_TOKEN="github_pat_11AABCCDDEE13802849209HD09283CDFFF" gitxray -r theredguild/devsecoops -l -v
```

You should see: `GitHub Token Loaded from GH_ACCESS_TOKEN env variable`, this means you're ready to go.

The first time I wanted to play with a tool like this one, was when I spent hours trying to understand what had happened with the xz backdoor, and how much involvement the infiltrator `JiaT75` had in it. The repo is: https://github.com/tukaani-project/xz.

Let's do a basic analysis.

```bash
wanderer@trg ~ $ gitxray -r tukaani-project/xz -l -v
...
Now verifying repository: tukaani-project/xz
Identified 54 contributors..                                                                      
LISTING CONTRIBUTORS (INCLUDING THOSE WITHOUT A GITHUB USER ACCOUNT) AND EXITING..
Larhzu, JiaT75, thesamesam, adrien-n, jrn, hansjans162, kiyolee, mvatsyk-lsg, afb, maan@tuebingen.mpg.de, bjarniig@rhi.hi.is, junghans, gabibguti, jmarrec, meyering@redhat.com, kianmeng, vapier, skosukhin, stoeckmann, jcallen, kilobyte, bluhm, svpv, radarhere, Coeur, mathstuf, bebuch, bensberg@justemail.net, ChanTsune, parheliamm, ivq, DimitriPapadopoulos, emaste, firasuke, hjl-tools, addaleax, iv-m, Jamaika1, jbastian, jiat75@gmail.com, meyering, vaeth@mathematik.uni-wuerzburg.de, nickajacks1, praiskup, proski@gnu.org, RainRat, ryandesign, sebastianas, vnwildman, scop, xry111, yarikoptic, biergaizi, huangqinjin
```

We can see JiaT75 appearing second. Let's add that username as a parameter to the previous command I'm going to skip to some of the most interesting parts I want to show you.

```plaintext
wanderer@trg ~ $ gitxray -r tukaani-project/xz -c JiaT75
....
Now verifying repository: tukaani-project/xz
YOU HAVE SCOPED THIS GITXRAY TO CONTRIBUTORS: ['JiaT75'] - OTHER USERS WON'T BE ANALYZED.
[comments]:   The Issues comment with the most NEGATIVE reactions (66) is available at: https://github.com/tukaani-project/xz/issues/92#issuecomment-2027786734
[branches]:   12 Unprotected Branches: ['build_werror', 'build_werror2', 'mask_cntrl_chars', 'master', 'memavail', 'single_stream_keeps', 'v5.0', 'v5.2', 'v5.4', 'v5.6', 'version_txt', 
[releases]:   User JiaT75 created historically 10 releases [71.43%] and uploaded a total of 60 assets [66.67%] Turn on Verbose mode for more information.
[contributors]: Top repository contributors with rejected PRs: [stoeckmann 2 rejected out of 2] | [thesamesam 2 rejected out of 2] | [skosukhin 2 rejected out of 2]
[contributors]: Top non-contributor GitHub users with rejected PRs: [brad0 2 rejected out of 2] | [ChenPi12 1 rejected out of 1] | [wzhengsen 1 rejected out of 1]
Some profiling:
[workflows]:  Workflow [CI] created [2022-12-22T13:37:40.000-03:00], updated [2022-12-22T13:37:40.000-03:00]: [https://github.com/tukaani-project/xz/blob/master/.github/workflows/ci.yml]
[workflows]:  Workflow [CI] has [121] missing or deleted runs. This could have been an attacker erasing their tracks, or legitimate cleanup. Set verbose mode for more data. 
[workflows]:  Workflow [CI] was run by [2] NON-contributors [2] times and by [7] contributors [7] times. Set verbose mode for more data. [https://github.com/tukaani-project/xz/actions/workflows/ci.yml]
```

And now specifically about JiaT75

```plaintext
Found results for account JiaT75.
[urls]:       Contributor URL: https://github.com/JiaT75
[urls]:       Owned repositories: https://github.com/JiaT75?tab=repositories
[personal]:   [Name: Jia Tan] obtained from the user's profile.
[emails]:     [jiat0218@gmail.com] obtained from the user's profile.
[emails]:     [jiat0218@gmail.com] obtained by parsing commits.
[profiling]:  Contributor account created: 2021-01-26T18:11:07Z, is 3.73 years old.
[profiling]:  The account was last updated at 2024-03-29T20:19:23Z, 202 days ago.
[profiling]:  Contributor has 10 total public repos.
[profiling]:  Contributor has 646 followers.
[profiling]:  The user submitted 29 Pull Requests out of which 5 were rejected.
[profiling]:  The user submitted 29 Pull Requests out of which 24 were merged.
[commits]:    Made (to this repo) 452 commits, first one at 2022-01-28T12:47:55Z and last one at 2024-03-25T17:50:02Z.
[commits]:    Commit Hours for [452] commits:
            [12pm UTC: 116 (25.66%)]
            [3pm UTC: 73 (16.15%)]
            [1pm UTC: 70 (15.49%)]
            [2pm UTC: 57 (12.61%)]
            [4pm UTC: 52 (11.50%)]
            [5pm UTC: 18 (3.98%)]
            [11am UTC: 16 (3.54%)]
            [8am UTC: 13 (2.88%)]
            [10am UTC: 7 (1.55%)]
            [2am UTC: 6 (1.33%)]
            [3am UTC: 5 (1.11%)]
            [1am UTC: 4 (0.88%)]
            [4am UTC: 4 (0.88%)]
            [7am UTC: 3 (0.66%)]
            [9am UTC: 3 (0.66%)]
            [5am UTC: 1 (0.22%)]
            [7pm UTC: 1 (0.22%)]
            [6am UTC: 1 (0.22%)]
            [12am UTC: 1 (0.22%)]
            [6pm UTC: 1 (0.22%)]
```

As you can see, with only some basic parameters we have a lot of data from which we can infer more information to our research. Like for example, where that person might live if their commits were pushed during the workday, or maybe after work.

### GitHub: Fake analyzer

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

### GitHub: Permissions

Sometimes using many repositories and participating in many organizations can be a bit tedious wheen trying to assess permissions. There are some tools that can check them out for you an elaborate a report and let you know where you are weak.

A tool that does this is Legitify. We're not doing a tutorial about this tool for the workshop because we'd like to focus on more things (and it's not that hard to use).

Also, we don't want you to be in a room fool of hackers trying to create a new access token with this level of permissions on the fly, we might even see you don't use 2FA to change security settings and feel our wrath.

## CI/CD

CI/CD pipelines automate software development but can be exploited if not secured. Attackers may inject malicious code, steal secrets, or escalate privileges, threatening the software supply chain. Securing these pipelines involves enforcing access controls, scanning for vulnerabilities, managing secrets, and integrating security checks to detect issues early and prevent attacks.

For this scenario we're going to use the repository `cicd-goat` from @cider-security-research.

```bash
git clone https://github.com/cider-security-research/cicd-goat
```

Since there are many ways on how you can inspect a vulnerable CI/CD, instead of deploying or running each challenge, (and because it is not our intention to exploit them) we are going just to scan them as an example of how some of these tools work.

In this case we're going to use `checkov`, since it has the possibility to scan many frameworks:

```plaintext
all, ansible, argo_workflows, arm, azure_pipelines, bicep, bitbucket_pipelines, cdk,
circleci_pipelines, cloudformation, dockerfile, github_configuration, github_actions, gitlab_configuration,
gitlab_ci, bitbucket_configuration, helm, json, yaml, kubernetes, kustomize, openapi, sca_package, sca_image,
secrets, serverless, terraform, terraform_json, terraform_plan, sast, sast_python, sast_java, sast_javascript,
sast_typescript, sast_golang, 3d_policy
```

To run it, just

```json
wanderer@trg ~ $ checkov -d cicd-goat/
[ arm framework ]: 100%|████████████████████|[44/44], Current File Scanned=../../../gite
[ dockerfile framework ]: 100%|████████████████████|[12/12], Current File Scanned=../../
[ kubernetes framework ]: 100%|████████████████████|[50/50], Current File Scanned=jenkin
[ terraform framework ]: 100%|████████████████████|[6/6], Current File Scanned=cicd-goat
[ secrets framework ]: 100%|████████████████████|[10/10], Current File Scanned=cicd-goat
[ gitlab_ci framework ]: 100%|████████████████████|[2/2], Current File Scanned=gitlab/repositories/awesome-app/.gitlab-ci.yml
[ circleci_pipelines framework ]: 100%|████████████████████|[1/1], Current File Scanned=.circleci/config.yml
[ github_actions framework ]: 100%|████████████████████|[41/41], Current File Scanned=.github/workflows/release.yaml
```

### GitHub Actions

There are many many ways you can check the security of your github actions, I'm going to leave you with some basic tools and possibly a few github actions that will hopefully get detected by them.

We are going to use GitHub Actions Goat, a deliberately vulnerable GH actions CI/CD environment, so we don't accidentally find vulnerabilities in live projects by accident :sweat:.

We're going to clone the repository:
`git clone https://github.com/step-security/github-actions-goat`

```bash
export PAT=github_pat_11ahshdasadasdkjasdhksajdsaj
set PAT pat github_pat_11AABCCDDEE13802849209HD09283CDFFF
```

```bash
gh-workflow-auditor --type repo step-security-github-actions-goat
```

`octoscan`

https://github.com/synacktiv/octoscan

```bash
wanderer@trg ~ $ octoscan dl --org theredguild --repo devsecops-toolkit --token github_pat_11AABCCDDEE13802849209HD09283CDFFF
```

Downloads all the branches' workflows to be analyzed locally.

```bash
wanderer@trg octoscan-output/DevSecOps-toolkit $ ls
ci-moar-space			os-motd
develop				os-pipx-path-and-shell-prompt
docker-compose			readme-docs-1
dockerfile-gosu			readme-todo-list
dockerfile-merge-runs		tool-2ms-and-zsh
docs-create-howto		tool-dockle
docs-readme-motd		tool-hadolint-kics
gha-tests			tool-semgrep-and-reorder
main				tools-clair
misc-dockerfile-make-docs	tools-fixes-1
misc-dockerfile-readme-1	tools-fixes-2
misc-hostname-motd		tools-scoutsuite-checkov-pmapper
multi-dockerfile-readme		tools-versions
multi-tools-pipx-git-asdf-npm	workshop-minimal
multi-tools-snyk-grype-and-more
```

So you can then run a scan directly to the output.

```bash
wanderer@trg ~ $ octoscan scan octoscan-output/theredguild/DevSecOps-toolkit
octoscan-output/theredguild/DevSecOps-toolkit/ci-moar-space/.github/workflows/test-tools.yml:47:27: Expression injection, "steps.**.outputs.**" is potentially untrusted. [expression-injection]
   |
47 |           build-args: ${{ steps.dotenv.outputs.vars }}
   |                           ^~~~~~~~~~~~~~~~~~~~~~~~~
```

Further reading:
- https://securitylab.github.com/resources/github-actions-preventing-pwn-requests/

## Secrets
Inside anything!

Semgrep.
Wrongsecrets.

## Source code
Introduction

Semgrep

### Dependencies

Depcheck. Depscan.

### NodeJS
Retire
Better npm audit
Nodejsscan

## Container / Image

`dockle`
`semgrep`