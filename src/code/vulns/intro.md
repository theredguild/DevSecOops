# Vulnerabilities

When it comes to vulnerabilities, it's a broad topic with many potential entry points within a project. Whether it's the code you wrote, contributions from others, or third-party libraries and dependencies, vulnerabilities can lurk anywhere.

In the `develop` branch of the @devsecops-toolkit, you'll find tools like [**`Grype`**](https://github.com/anchore/grype) and [**`Snyk`**](https://github.com/snyk/cli) that are designed to scan for vulnerabilities within a repository. However, our focus will be on exploring other tools that can enhance your security arsenal.

For those interested in expanding their toolkit, the [OWASP's Source Code Analysis Tools](https://owasp.org/www-community/Source_Code_Analysis_Tools) list is a great resource to explore.

As part of our hands-on testing, we'll be directing these tools towards the NodeJS GOAT repository to see them in action.

```bash
git clone https://github.com/OWASP/NodeGoat
cd NodeGoat
npm install
```

The installation process will take some time. But you can even tell by the looks of npm's install output that this repository isn't any good.

```json
added 962 packages, and audited 1412 packages in 4m

130 vulnerabilities (5 low, 38 moderate, 55 high, 32 critical)
```
