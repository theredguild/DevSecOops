# 2ms

What we like about 2ms is that it will allow you to scan through different platforms that we haven't seen in other tools, such as: Cnfluence, Discord, and Slack servers; Paligo instances; git repositories and filesystem files/folders.

Everytime you see filesystem and local git repository as options you might think it is the same, but don't forget patterns may benefit one scenario more than other.

Here you can see a difference.

**Filesystem module:**

```bash
wanderer@trg ~ $ 2ms filesystem --path fake-leaks
totalitemsscanned: 205
totalsecretsfound: 584
```

**Git module:**

```bash
wanderer@trg ~ $ 2ms git fake-leaks | less
totalitemsscanned: 275
totalsecretsfound: 649
...
```

A more obvious example when it comes to deciding whether to pick is the following one, and we think with this one you'll get it.

Leaving only the `.git` folder from the repo:

```bash
wanderer@trg fake-leaks-git $ ls -a
.  ..  .git
```

**Filesystem module:**

```bash
wanderer@trg ~ $ 2ms filesystem --path fake-leaks-git
18:23:46 INF Folder plugin started
18:23:46 INF Scan completed with empty content
```

**Git module:**

```bash
wanderer@trg ~ $ 2ms git fake-leaks-git
18:25:12 INF Git plugin started
totalitemsscanned: 275
totalsecretsfound: 649
```

And it will be a little more obvious when you check the sources of the information from where the secret was obtained. In the case of filesystem they will always come from paths, like `source: fake-leaks/test/recipes/30-test_evp_data/evppkey_rsa_common.txt`, and for the git module they may come from things like `git show c4091a5371939a8471a7cfef58497377cb4ec375:specs/github.json`.

