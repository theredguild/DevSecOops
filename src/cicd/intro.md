# CI/CD

CI/CD pipelines automate software development but can be exploited if not secured. Attackers may inject malicious code, steal secrets, or escalate privileges, threatening the software supply chain. Securing these pipelines involves enforcing access controls, scanning for vulnerabilities, managing secrets, and integrating security checks to detect issues early and prevent attacks.

For this scenario we're going to use the repository `cicd-goat` from @cider-security-research.

```bash
git clone https://github.com/cider-security-research/cicd-goat
```

Since there are many ways on how you can inspect a vulnerable CI/CD, instead of deploying or running each challenge, and because it is not our intention to exploit them, we are going just to scan them to understand how some of these tools work.
