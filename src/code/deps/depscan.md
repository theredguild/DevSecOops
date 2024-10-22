# OWASP's dep-scan

So, about [this first one](https://github.com/owasp-dep-scan/depscan-bin/), it is actually an official project from OWASP. It focused on known vulnerabilities, advisories, and license restrictions in project dependencies. It can process both local repositories and container images as input, making it perfect for integration.

We are going to show you many tools who have features, based on other projects like [cdxgen](https://github.com/CycloneDX/cdxgen), to generate Software Bill-of-Materials (BOM). This is like basically creating a list of all the libraries, dependencies, configurations, workflows, your project has in order to get them analyzed.

This is a table showing that it currently supports at the time of writing this document:

| Language/Platform          | Files/Formats                                                                 |
|----------------------------|-------------------------------------------------------------------------------|
| node.js                    | package-lock.json, pnpm-lock.yaml, yarn.lock, rush.js, bower.json, .min.js    |
| java                       | maven (pom.xml [1]), gradle (build.gradle, .kts), scala (sbt), bazel          |
| php                        | composer.lock                                                                 |
| python                     | setup.py, requirements.txt [2], Pipfile.lock, poetry.lock, bdist_wheel, .whl, .egg-info |
| go                         | binary, go.mod, go.sum, Gopkg.lock                                            |
| ruby                       | Gemfile.lock, gemspec                                                         |
| rust                       | binary, Cargo.toml, Cargo.lock                                                |
| .Net                       | .csproj, packages.config, project.assets.json [3], packages.lock.json, .nupkg |
| dart                       | pubspec.lock, pubspec.yaml                                                    |
| haskell                    | cabal.project.freeze                                                          |
| elixir                     | mix.lock                                                                      |
| c/c++                      | conan.lock, conanfile.txt                                                     |
| clojure                    | Clojure CLI (deps.edn), Leiningen (project.clj)                               |
| docker / oci image         | All supported languages and Linux OS packages                                 |
| GitHub Actions Workflows   | .github/workflows/*.yml                                                       |
| Jenkins Plugins            | .hpi files                                                                    |
| YAML manifests             | docker-compose, kubernetes, kustomization, skaffold, tekton etc               |

### Performing a simple scan

I added the `explain` argument to be able to have a beautiful output and paste it here.

```bash
wanderer@trg NodeGoat $ depscan --src $PWD --explain -o reports/nodegoat.json

██████╗ ███████╗██████╗ ███████╗ ██████╗ █████╗ ███╗   ██╗
██╔══██╗██╔════╝██╔══██╗██╔════╝██╔════╝██╔══██╗████╗  ██║
██║  ██║█████╗  ██████╔╝███████╗██║     ███████║██╔██╗ ██║
██║  ██║██╔══╝  ██╔═══╝ ╚════██║██║     ██╔══██║██║╚██╗██║
██████╔╝███████╗██║     ███████║╚██████╗██║  ██║██║ ╚████║
╚═════╝ ╚══════╝╚═╝     ╚══════╝ ╚═════╝╚═╝  ╚═╝╚═╝  ╚═══╝

INFO [2024-10-21 15:33:07,496] To improve performance, cache the bom file and invoke depscan with --bom reports/nodegoat-universal.json instead of -i
INFO [2024-10-21 15:33:13,484] Performing regular scan for /home/wanderer/NodeGoat using plugin universal

                                                       Dependency Scan Results (UNIVERSAL)
╔════════════════════════════════════════════════════════════════╤══════════════════════════════════╤══════════════════╤══════════════╤═════════╗
║ CVE                                                            │ Insights                         │ Fix Version      │ Severity     │   Score ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────
║ prettyjson@1.2.1                                                  │ 📓 Indirect dependency         │ 1.2.6            │ CRITICAL    │     9.0 ║
║ └── minimist@1.2.5 ⬅ CVE-2021-44906                               │                                │                  │             │         ║
╟───────────────────────────────────────────────────────────────────┼────────────────────────────────┼──────────────────┼─────────────┼─────────╢
║ zaproxy@0.2.0                                                     │ 📓 Indirect dependency         │ 4.17.12          │ CRITICAL    │     9.0 ║
║ └── lodash@2.4.2 ⬅ CVE-2019-10744                                 │                                │                  │             │         ║
╟───────────────────────────────────────────────────────────────────┼────────────────────────────────┼──────────────────┼─────────────┼─────────╢
║ shelljs@0.3.0 ⬅ NPM-1088208                                    │ 📓 Indirect dependency           │ 0.8.5            │ MEDIUM       │     5.0 ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────╢
║ growl@1.9.2 ⬅ CVE-2017-16042                                   │ 📓 Indirect dependency           │ 1.10.0           │ CRITICAL     │     9.0 ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────╢
║ fsevents@1.2.9 ⬅ NPM-1091853                                   │ 📓 Indirect dependency           │ 1.2.11           │ CRITICAL     │     9.0 ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────╢
║ bson@1.0.9 ⬅ CVE-2019-2391                                     │ 📓 Indirect dependency           │ 1.1.4            │ MEDIUM       │     5.0 ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────╢
║ ini@1.3.4 ⬅ CVE-2020-7788                                      │ 📓 Indirect dependency           │ 1.3.6            │ HIGH         │     7.5 ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────╢
║ getobject@0.1.0 ⬅ CVE-2020-28282                               │ 📓 Indirect dependency           │ 1.0.0            │ CRITICAL     │     9.0 ║
╟────────────────────────────────────────────────────────────────┼──────────────────────────────────┼──────────────────┼──────────────┼─────────╢
║ node@12-alpine ⬅ CVE-2022-0155                                 │ 📔 Has PoC                       │                  │ MEDIUM       │     6.5 ║
║                                                                │ 🎯 Distro specific               │                  │              │         ║
╚════════════════════════════════════════════════════════════════╧══════════════════════════════════╧══════════════════╧══════════════╧═════════╝
╭──────────────────────────────────── Recommendation ────────────────────────────────────╮
│ 👉 6 out of 206 vulnerabilities requires your attention.                               │
│ You can remediate 70 vulnerabilities by updating the packages using the fix version 👍 │
╰────────────────────────────────────────────────────────────────────────────────────────╯
```

### Risk audit and dependency confusion

There's a `--risk-audit` argument that enables package risk audit, currenlty working for npm and pypi packages. It weights several risk factors to compute a final score and shows it only if that score surpasses a certain configurable threshold. You can check out how to configure the weights on [their docs](https://owasp.org/www-project-dep-scan/).

There is also the possibility to use `--private-ns` to specify the private package namespace that should be checked for dependency confusion type issues where a private package is available on public npm/pypi registry. This is a very common attack vector.

If we were to run a risk audit, with a dependency confusion check, and change a few weights:

```bash
wanderer@trg NodeGoat $ PKG_INSTALL_SCRIPTS=4 DEPRECATED=0 depscan --src . --risk-audit --private-ns theredguild
```

If you pay attention, you can see how risk scores have changed thanks to our tweaks.

### Live Operating System Scan

One of the interesting features it has, is the Live OS scan.

By passing `-t os`, depscan can generate an SBoM for a live operating system or a VM with OS packages and kernel information. Optionally, pass the argument `--deep` to generate an SBoM with both OS and application packages and to check for application vulnerabilities.

We're going to show you an excerpt of a live os scan from the container you're currently working on.

```bash
wanderer@trg ~ $ depscan -t os --deep -i . -o reports/fullos.json --explain

    ██████╗ ███████╗██████╗ ███████╗ ██████╗ █████╗ ███╗   ██╗
    ██╔══██╗██╔════╝██╔══██╗██╔════╝██╔════╝██╔══██╗████╗  ██║
    ██║  ██║█████╗  ██████╔╝███████╗██║     ███████║██╔██╗ ██║
    ██║  ██║██╔══╝  ██╔═══╝ ╚════██║██║     ██╔══██║██║╚██╗██║
    ██████╔╝███████╗██║     ███████║╚██████╗██║  ██║██║ ╚████║
    ╚═════╝ ╚══════╝╚═╝     ╚══════╝ ╚═════╝╚═╝  ╚═╝╚═╝  ╚═══╝

INFO [2024-10-21 15:10:27,649] About to perform deep scan. This could take a while ...
INFO [2024-10-21 15:11:06,152] To improve performance, cache the bom file and invoke depscan with --bom reports/fullos-os.json instead of -i
INFO [2024-10-21 15:11:06,193] About to download the vulnerability database from ghcr.io/appthreat/vdbgz:v5. This might take a while ...
INFO [2024-10-21 15:17:08,655] Performing regular scan for /home/wanderer using plugin os

                                                          Dependency Scan Results (OS)
╔═══════════════════════════════════════════════════════════════╤═══════════════════════════════╤═══════════════════════════╤═══════════╤═══════╗
║ CVE                                                           │ Insights                      │ Fix Version               │ Severity  │ Score ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ curl@7.88.1-10+deb12u7 ⬅ CVE-2024-6197                        │ ✂ Uninstall candidate         │ 8.9.0                     │ HIGH      │   7.5 ║
║                                                               │ 📓 Local install              │                           │           │       ║
║                                                               │ 📔 Has PoC                    │                           │           │       ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ libpython3.11@3.11.2-6+deb12u3 ⬅ CVE-2024-6923                │ 📓 Local install              │                           │ MEDIUM    │   6.8 ║
║                                                               │ 🎯 Distro specific            │                           │           │       ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ pyyaml@6.0 ⬅ CVE-2020-1747                                    │ 🧾 Vendor Confirmed           │ 5.4                       │ CRITICAL  │   9.8 ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ curl@7.88.1-10+deb12u7 ⬅ CVE-2023-27538                       │ ✂ Uninstall candidate         │ 8.0.0                     │ MEDIUM    │   5.5 ║
║                                                               │ 📓 Local install              │                           │           │       ║
║                                                               │ 📔 Has PoC                    │                           │           │       ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ jinja2@3.1.2 ⬅ CVE-2024-34064                                 │ 🧾 Vendor Confirmed           │ 3.1.4                     │ MEDIUM    │   5.4 ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ libhogweed6@3.8.1-2 ⬅ CVE-2023-36660                          │ 📓 Local install              │ 3.9.1-2.1                 │ MEDIUM    │   5.0 ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ fdupes@1:2.2.1-1 ⬅ CVE-2022-48682                             │ 📓 Local install              │ 2.2.1-1                   │ MEDIUM    │   5.0 ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ vim@2:9.0.1378-2 ⬅ CVE-2023-48233                             │ ✂ Uninstall candidate         │ 9.0.2116-1                │ LOW       │   2.0 ║
║                                                               │ 📓 Local install              │                           │           │       ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ systemd@252.30-1~deb12u2 ⬅ CVE-2023-31437                     │ 🔇 Suppress for containers    │                           │ LOW       │   2.0 ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ gnupg@2.2.40-1.1 ⬅ CVE-2022-3515                              │ 📓 Local install              │ 2.2.41                    │ CRITICAL  │   9.8 ║
╟───────────────────────────────────────────────────────────────┼───────────────────────────────┼───────────────────────────┼───────────┼───────╢
║ vim-common@2:9.0.1378-2 ⬅ CVE-2023-5441                       │                               │ 9.0.2010                  │ LOW       │   2.0 ║
╚═══════════════════════════════════════════════════════════════╧═══════════════════════════════╧═══════════════════════════╧═══════════╧═══════╝

                                                                   Next Steps

Below are the vulnerabilities prioritized by depscan. Follow your team's remediation workflow to mitigate these findings.

                                                                Top Priority (OS)
╔═══════════════════════════════════════════════════════════════════════╤═══════════════════════════╤═══════════════════════╤═══════════════════╗
║ Package                                                               │ CVEs                      │ Fix Version           │ Reachable         ║
╟───────────────────────────────────────────────────────────────────────┼───────────────────────────┼───────────────────────┼───────────────────╢
║ setuptools@66.1.1 ⬅ CVE-2024-6345                                     │ CVE-2024-6345             │ 70.0.0                │                   ║
╟───────────────────────────────────────────────────────────────────────┼───────────────────────────┼───────────────────────┼───────────────────╢
║ curl@7.88.1-10+deb12u7 ⬅ CVE-2024-6197                                │ CVE-2024-6197             │ 8.9.0                 │                   ║
╚═══════════════════════════════════════════════════════════════════════╧═══════════════════════════╧═══════════════════════╧═══════════════════╝

╭──────────────────────────────────── Recommendation ────────────────────────────────────╮
│ 👉 1 out of 468 vulnerabilities requires your attention.                               │
│ You can remediate 67 vulnerabilities by updating the packages using the fix version 👍 │
╰────────────────────────────────────────────────────────────────────────────────────────╯
```

We've seen enough of this tool! Let's check out the following one.
