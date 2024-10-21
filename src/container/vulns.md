# Vulnerabilities

Container security encompasses more than just code vulnerabilities; it includes configurations, dependencies, and runtime behaviors. Here are key tools to help secure your containers:

- **Trivy**: A comprehensive scanner for vulnerabilities, misconfigurations, secrets, and SBOMs in containers, Kubernetes, and more. It's fast and user-friendly, making it ideal for quick security assessments.

- **Clair**: Conducts static analysis of container images, identifying vulnerabilities by comparing them against a database. It's well-suited for CI/CD integration to ensure images are secure before deployment.

- **Falco**: Offers runtime security by monitoring container behavior for anomalies. It alerts on suspicious activities, making it crucial for real-time security monitoring.

- **Snyk**: Specializes in open source dependencies, container images, and Kubernetes security. It integrates with development workflows, providing actionable insights and automated fixes, ideal for DevOps teams.

- **Grype**: A straightforward tool for scanning container images for vulnerabilities. It provides detailed reports and is easily integrated into CI/CD pipelines for automated checks.

- **Checkov**: A static code analysis tool for infrastructure as code (IaC). It helps detect misconfigurations in container orchestration files, ensuring compliance and security best practices.

Each tool has its strengths: Trivy and Grype for scanning, Clair for static analysis, Falco for runtime monitoring, Snyk for development integration, and Checkov for IaC security. Choose based on your specific security needs and workflow requirements.

They have all been integrated into the `develop` branch of **@theredguild/devsecops-toolkit**. However, for the `minimal-workshop`, we have only included Trivy and Checkov. Since Checkov needs a Bridgecrew platform API key to scan images, and we've already used it once, we are going to focus on Trivy.

## Trivy

Trivy is an exceptional tool, we can't explain how many features, and configurations it has. You should check it out by yourself. For example, look at the parameters you can pass to the `image` command. It even comes with great examples.

```bash
trivy image
...

Examples:
  # Scan a container image
  $ trivy image python:3.4-alpine

  # Scan a container image from a tar archive
  $ trivy image --input ruby-3.1.tar

  # Filter by severities
  $ trivy image --severity HIGH,CRITICAL alpine:3.15

  # Ignore unfixed/unpatched vulnerabilities
  $ trivy image --ignore-unfixed alpine:3.15

  # Scan a container image in client mode
  $ trivy image --server http://127.0.0.1:4954 alpine:latest

  # Generate json result
  $ trivy image --format json --output result.json alpine:3.15

  # Generate a report in the CycloneDX format
  $ trivy image --format cyclonedx --output result.cdx alpine:3.15
```

Normally, you would use trivy to scan images without the need to input a file. But since we don´t want to run docker inside this container, you can feed it an image file.

```bash
wanderer@trg DevSecOps-toolkit $ trivy image --input python.tar
2024-10-21T22:37:58Z	INFO	[vuln] Vulnerability scanning is enabled
2024-10-21T22:37:58Z	INFO	[secret] Secret scanning is enabled
2024-10-21T22:37:58Z	INFO	[secret] If your scanning is slow, please try '--scanners vuln' to disable secret scanning
2024-10-21T22:37:58Z	INFO	[secret] Please see also https://aquasecurity.github.io/trivy/v0.56/docs/scanner/secret#recommendation for faster secret detection
2024-10-21T22:37:58Z	INFO	Detected OS	family="alpine" version="3.9.2"
2024-10-21T22:37:58Z	INFO	[alpine] Detecting vulnerabilities...	os_version="3.9" repository="3.9" pkg_num=28
2024-10-21T22:37:58Z	INFO	Number of language-specific files	num=1
2024-10-21T22:37:58Z	INFO	[python-pkg] Detecting vulnerabilities...
2024-10-21T22:37:58Z	WARN	This OS version is no longer supported by the distribution	family="alpine" version="3.9.2"
2024-10-21T22:37:58Z	WARN	The vulnerability detection may be insufficient because security updates are not provided

python.tar (alpine 3.9.2)

Total: 3 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 1, CRITICAL: 2)

┌──────────────┬────────────────┬──────────┬────────┬───────────────────┬───────────────┬────────────────────────────────────────────────────────���─────┐
│   Library    │ Vulnerability  │ Severity │ Status │ Installed Version │ Fixed Version │                            Title                             │
├──────────────┼────────────────┼──────────┼────────┼───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ expat        │ CVE-2018-20843 │ HIGH     │ fixed  │ 2.2.6-r0          │ 2.2.7-r0      │ expat: large number of colons in input makes parser consume  │
│              │                │          │        │                   │               │ high amount...                                               │
│              │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2018-20843                   │
├──────────────┼────────────────┼──────────┤        ├───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libbz2       │ CVE-2019-12900 │ CRITICAL │        │ 1.0.6-r6          │ 1.0.6-r7      │ bzip2: out-of-bounds write in function BZ2_decompress        │
│              │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2019-12900                   │
├──────────────┼────────────────┼──────────┤        ├───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ musl         │ CVE-2019-14697 │ CRITICAL │        │ 1.1.20-r4         │ 1.1.20-r5     │ musl libc through 1.1.23 has an x87 floating-point stack     │
│              │                │          │        │                   │               │ adjustment im ......                                         │
│              │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2019-14697                   │
└──────────────┴────────────────┴──────────┴────────┴───────────────────┴───────────────┴──────────────────────────────────────────────────────────────┘
2024-10-21T22:37:58Z	INFO	Table result includes only package filenames. Use '--format json' option to get the full path to the package file.
```

And here the results for `python-pkg`. I trimmed and separated them as usual.

```
Python (python-pkg)

Total: 6 (UNKNOWN: 0, LOW: 0, MEDIUM: 1, HIGH: 5, CRITICAL: 0)

┌───────────────────────┬────────────────┬──────────┬────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
│        Library        │ Vulnerability  │ Severity │ Status │ Installed Version │ Fixed Version │                            Title                            │
├───────────────────────┼────────────────┼──────────┼────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ pip (METADATA)        │ CVE-2019-20916 │ HIGH     │ fixed  │ 19.0.3            │ 19.2          │ python-pip: directory traversal in _download_http_url()     │
│                       │                │          │        │                   │               │ function in src/pip/_internal/download.py                   │
│                       │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2019-20916                  │
│                       ├────────────────┤          │        │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2021-3572  │          │        │                   │ 21.1          │ python-pip: Incorrect handling of unicode separators in git │
│                       │                │          │        │                   │               │ references                                                  │
│                       │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2021-3572                   │
│                       ├────────────────┼──────────┤        │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2023-5752  │ MEDIUM   │        │                   │ 23.3          │ pip: Mercurial configuration injectable in repo revision    │
│                       │                │          │        │                   │               │ when installing via pip                                     │
│                       │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2023-5752                   │
├───────────────────────┼────────────────┼──────────┤        ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ setuptools (METADATA) │ CVE-2022-40897 │ HIGH     │        │ 40.8.0            │ 65.5.1        │ pypa-setuptools: Regular Expression Denial of Service       │
│                       │                │          │        │                   │               │ (ReDoS) in package_index.py                                 │
│                       │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2022-40897                  │
│                       ├────────────────┤          │        │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-6345  │          │        │                   │ 70.0.0        │ pypa/setuptools: Remote code execution via download         │
│                       │                │          │        │                   │               │ functions in the package_index module in...                 │
│                       │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-6345                   │
├───────────────────────┼────────────────┤          │        ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ wheel (METADATA)      │ CVE-2022-40898 │          │        │ 0.33.1            │ 0.38.1        │ python-wheel: remote attackers can cause denial of service  │
│                       │                │          │        │                   │               │ via attacker controlled input...                            │
│                       │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2022-40898                  │
└───────────────────────┴────────────────┴──────────┴────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────────────┘
```
