# Container / Images

First, we have to do a proper differentiation about the various methods to identify and address security issues and misconfigurations in containers, along with examples of how to retrieve or generate each case.

1. **Live Container Scanning**: This involves scanning the container while it is running, similar to how you would perform security checks on a live operating system. For example, you can use a tool from within the container, running it locally systerm wide.

2. **Software Bill of Materials (SBOM) Analysis**: By extracting and analyzing the SBOM, you can gain insights into the components and dependencies within a container. For instance, you can generate an SBOM using `syft` with the command: `syft <image-name> -o json > sbom.json`.

3. **Image Layer Analysis**: To save a container image and its layers to disk, you can use Docker's `save` command. This command exports the image and its layers into a tarball, which can then be examined in detail. For example, you can save an image with the command: `docker save <image-name> -o <image-name>.tar`. Once saved, you can use tools like `dive` to analyze the image layers by first loading the tarball with `docker load -i <image-name>.tar` and then running `dive <image-name>`.

4. **Remote Container Scanning**: This approach involves scanning containers that are hosted remotely. For example, you can use `trivy` to scan a remote container image with the command: `trivy image theredguild/sampleRepo:latest`.

5. **Repository Scanning**: Scanning a repository that contains container images or Dockerfiles can help identify security issues before the containers are built and deployed. You can use `semgrep` to scan a local repository with the command: `semgrep --config "p/dockerfile" --path localRepo`.

Now, let's take a more practical approach to see these examples better.
