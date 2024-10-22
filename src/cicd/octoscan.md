# GitHub: Octoscan Static Vuln Scanner

There's also another interesting tool we've found worth mentioning, and it is [**`Octoscan`**](https://github.com/synacktiv/octoscan).

It has two commands, `dl` (download) and `scan`. Let's start with the first one:

```bash
wanderer@trg ~ $ octoscan dl --org theredguild --repo devsecops-toolkit --token github_pat_11AABCCDDEE13802849209HD09283CDFFF
```

If you check your current working directory, you'll see an output folder, where it as inclided for the target repo all the branches' workflows to be analyzed locally.

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

In our case, I know the workflow for each branch is almost the same, except for a few. In order to reduce redundancy, you can delete duplicated workflows by using the `fdupes` command first.

```bash
fdupes -n -r -N -d path/to/repo
```

Now, you can then run a scan directly to the output. Note that if you already have the repository locally, there's no need to download it like we did.

```bash
wanderer@trg ~ $ octoscan scan octoscan-output/theredguild/DevSecOps-toolkit
octoscan-output/theredguild/DevSecOps-toolkit/ci-moar-space/.github/workflows/test-tools.yml:47:27: Expression injection, "steps.**.outputs.**" is potentially untrusted. [expression-injection]
   |
47 |           build-args: ${{ steps.dotenv.outputs.vars }}
   |                           ^~~~~~~~~~~~~~~~~~~~~~~~~
```

You can check all the rules being used by running `octoscan scan --list-rules`, so you may disable the ones you don't need with `--disable rules`. Their documentation suggests to use this command as a first one when you don't know where to start:

```bash
octoscan scan path/to/repos/ --disable-rules shellcheck,local-action --filter-triggers external
```

The [README](https://github.com/synacktiv/octoscan/blob/master/README.md) from Octoscan provides a great explanation with real examples from each of the rules it uses.

There is also a [great guide](https://securitylab.github.com/resources/github-actions-preventing-pwn-requests/) by the Security Lab at GitHub in order to prevent what they call `pwn requests`.

If you want to add an action that proportionates a wide range of security measures, you can start by installing [HardenRunner](https://github.com/marketplace/actions/harden-runner). It provides runtime security for GitHub-hosted and self-hosted runners:

```yml
- name: Harden-Runner
  uses: step-security/harden-runner@v2.10.1
```
