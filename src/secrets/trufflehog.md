# Trufflehog

To start with [**this tool**](https://github.com/trufflesecurity/trufflehog)'s wizard you can run `sudo trufflehog` and follow the steps! But if you want a specific command, you can run each of them manually. Check `--help` to see them all. You can scan from git, to s3/gcs buckets; docker images, CIs, and even your filesystem.

Note: Make sure to append `--no-update` in order to avoid getting an error while updating to latest version. If you'd like to update you'll need to have root privileges or run it with `sudo`.

To start, you can start a local folder scan using the filesystem feature aimed at the repository we already downloaded. But we warn you, it will show you a lot of output...

```bash
trufflehog --no-update filesystem fake-leaks/
```

If you want to try a smaller scope, and scan a remote git repository, you can try @trufflesecurity's `test_keys` repository.

```bash
trufflehog --no-update git https://github.com/trufflesecurity/test_keys --only-verified
🐷🔑🐷  TruffleHog. Unearth your secrets. 🐷🔑🐷
...
✅ Found verified result 🐷🔑
Detector Type: URI
Decoder Type: PLAIN
Raw result: https://admin:admin@the-internet.herokuapp.com
Commit: 77b2a3e56973785a52ba4ae4b8dac61d4bac016f
Email: counter <counter@counters-MacBook-Air.local>
File: keys
Line: 3
Repository: https://github.com/trufflesecurity/test_keys
Timestamp: 2022-06-16 17:27:56 +0000
...
```

With Trufflehog you can even scan: Docker images, Travis/Circle CI, Syslog, S3/GCS buckets, Jenkins, and more.
