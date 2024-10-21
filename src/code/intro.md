# Source code

As we've discussed in the **Secrets** category, the code we write might inadvertently contain secrets, such as credentials or API keys. These can easily go unnoticed, especially after thoroughly debugging a part of an application, which might lead us to forget that we hardcoded sensitive information. Although this is a poor practice, it can happen when collaborating with many people or receiving contributions from external parties who may not be as experienced.

However, secrets are not the only concern. There are also clear vulnerable patterns and well-known vulnerabilities that can be addressed before the code goes into production.

In this section, we will explore a set of tools designed to help us prevent supply-chain attacks by checking dependencies, detecting publicly disclosed vulnerabilities, and more.
