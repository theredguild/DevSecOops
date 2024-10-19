# Secrets

Secrets, passwords, credentials... you name it. They are everywhere because they are needed. Tokens, API keys, private keys leaks and more are the sole reason of why some projects get rekt. They keep getting pushed to repositories accidentally, or sometimes a colleage pasted it in a Discord channel thinking it might not harm, until someone does a takeover and things scalate quickly.

For this section, we're gonna rely on two repositories. One which aims to be interactive, and also has a [CTF demo](https://wrongsecrets.herokuapp.com/) in case you don't want to set-up your own, which is [wrongsecrets](https://github.com/OWASP/wrongsecrets) by OWASP. The other one is [fake-leaks](https://github.com/leaktk/fake-leaks) by a recent project that aims to integrate all secret scanning tools there are available.

There are many, _many_ tools to scan for secrets, and many ways you can do it. You can scan your code, you can scan your IaC, you can scan your whole repository, or you can even have proactive measures. Things that hook to git commands and prevent you from committing them in the first place. And in case you missed anything, or someone is leaking their credentials through a PR, you can even trigger actions afterward.
