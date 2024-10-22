# Permissions

Sometimes using many repositories and participating in several organizations can be a bit tedious wheen trying to assess permissions. There are some tools that can check them out for you and elaborate a report to let you know where you are weak or someone could leverage permissions.

A tool that does this is [Legitify](https://github.com/Legit-Labs/legitify). We're not doing a tutorial about this tool for the workshop because we'd like to focus on more things (and it's not that hard to use).

Also, we don't want you to be in a room fool of hackers trying to create a new access token with this level of permissions on the fly, we might even see you don't use 2FA to change security settings and feel our wrath.

Other tools you could use:

```bash
trufflehog --no-update analyze # and select git/github, pasting your PAT
```
