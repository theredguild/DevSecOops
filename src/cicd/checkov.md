# Checkov

In this case we're going to use **[`checkov`](https://github.com/bridgecrewio/checkov)**, since it has the possibility to scan many frameworks:

```plaintext
all, ansible, argo_workflows, arm, azure_pipelines, bicep, bitbucket_pipelines, cdk,
circleci_pipelines, cloudformation, dockerfile, github_configuration, github_actions, gitlab_configuration,
gitlab_ci, bitbucket_configuration, helm, json, yaml, kubernetes, kustomize, openapi, sca_package, sca_image,
secrets, serverless, terraform, terraform_json, terraform_plan, sast, sast_python, sast_java, sast_javascript,
sast_typescript, sast_golang, 3d_policy
```

We're going to run it without further configuration just select a folder:

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
... continues
```

You can see the progress bar for each framework being scanned, and then you're going to see how many passed the tests or failed them. In this scenario there's an overload of information given we just fed it a LOT of vulnerable cases.

To avoid this, something you could do is select the quiet and compact view of the output.

```bash
checkov -d cicd-goat --quiet --compact
(trimming some warnings)

terraform scan results:
Passed checks: 32, Failed checks: 19, Skipped checks: 0

Check: CKV_GLB_1: "Ensure at least two approving reviews are required to merge a GitLab MR"
	FAILED for resource: gitlab_project.pygryphon_project
	File: /gitlab/gitlab.tf:73-79
	Guide: https://docs.prismacloud.io/en/enterprise-edition/policy-reference/build-integrity-policies/gitlab-policies/merge-requests-do-not-require-two-or-more-approvals-to-merge
Check: CKV_GLB_4: "Ensure GitLab commits are signed"
...

dockerfile scan results:
Passed checks: 652, Failed checks: 26, Skipped checks: 0

Check: CKV_DOCKER_1: "Ensure port 22 is not exposed"
	FAILED for resource: /prod/Dockerfile.EXPOSE
	File: /prod/Dockerfile:33-33
	Guide: https://docs.prismacloud.io/en/enterprise-edition/policy-reference/docker-policies/docker-policy-index/ensure-port-22-is-not-exposed
...
continues
```

If you disable `compact`, you will be able to see the vulnerable lines that made the test fail.

And there's much more that you can do, this tools scans for Docker images/files, secrets, and even allows you to plug-in OpenAI to improve the explanations.

Here's an example:

```bash
wanderer@trg ~ $ checkov -d cicd-goat/gitlab-runner/ --quiet --openai-api-key sk-3QqBtfJ...TUV-OWbB
WARNING: About to request 2 enhanced guidelines and it may take up to 15s.

dockerfile scan results:
Passed checks: 31, Failed checks: 2, Skipped checks: 0

Check: CKV_DOCKER_2: "Ensure that HEALTHCHECK instructions have been added to container images"
	FAILED for resource: /Dockerfile.
	Details: The following text is AI generated and should be treated as a suggestion.
	
	         The issue with the provided Dockerfile is that it does not include a HEALTHCHECK instruction.
	         According to the checkov policy 'Ensure that HEALTHCHECK instructions have been added to container images', it is recommended to include a HEALTHCHECK instruction in Dockerfiles to monitor the status of a container.
	
	         To fix this issue, you can add a HEALTHCHECK instruction to the Dockerfile.
	         Here is an example of how you can add a simple HEALTHCHECK instruction to the Dockerfile:
	
	         FROM docker:20.10.21-dind
	         ARG VERSION
	         ARG COMMIT_SHA
	
	         LABEL org.opencontainers.image.vendor="Cider Security" \
	             org.opencontainers.image.title="CI/CD Goat - GitLab Runner" \
	             org.opencontainers.image.description="Deliberately vulnerable CI/CD environment." \
	             org.opencontainers.image.url="https://hub.docker.com/r/cidersecurity/goat-gitlab-runner" \
	             org.opencontainers.image.source="https://github.com/cider-security-research/cicd-goat" \
	             org.opencontainers.image.licenses="Apache-2.0" \
	             org.opencontainers.image.version=$VERSION \
	             org.opencontainers.image.revision=$COMMIT_SHA
	
	         RUN apk add --no-cache gitlab-runner curl bash
	         COPY . /setup/
	         ENTRYPOINT ["/setup/run.sh"]
	
	         HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD curl -f http://localhost/ || exit 1
	
	         In this example, a simple HTTP health check is added using the `curl` command to check if the container is healthy.
	         You can customize the HEALTHCHECK instruction based on your specific requirements and the service running inside the container.
	File: /Dockerfile:1-16
	Guide: https://docs.prismacloud.io/en/enterprise-edition/policy-reference/docker-policies/docker-policy-index/ensure-that-healthcheck-instructions-have-been-added-to-container-images

		1  | FROM docker:20.10.21-dind
		2  | ARG VERSION
		3  | ARG COMMIT_SHA
		4  |
		5  | LABEL org.opencontainers.image.vendor="Cider Security" \
		6  |     org.opencontainers.image.title="CI/CD Goat - GitLab Runner" \
		7  |     org.opencontainers.image.description="Deliberately vulnerable CI/CD environment." \
		8  |     org.opencontainers.image.url="https://hub.docker.com/r/cidersecurity/goat-gitlab-runner" \
		9  |     org.opencontainers.image.source="https://github.com/cider-security-research/cicd-goat" \
		10 |     org.opencontainers.image.licenses="Apache-2.0" \
		11 |     org.opencontainers.image.version=$VERSION \
		12 |     org.opencontainers.image.revision=$COMMIT_SHA
		13 |
		14 | RUN apk add --no-cache gitlab-runner curl bash
		15 | COPY . /setup/
		16 | ENTRYPOINT ["/setup/run.sh"]
```

We will leave secrets and container image scanning example to other tools.
