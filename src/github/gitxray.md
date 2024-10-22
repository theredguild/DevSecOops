# GitXRay

One tool that works out of the box is **[`gitxray`](https://gitxray.com/)**, by a fellow hacker from the @ekoparty, Lucas.
The only thing I'd suggest doing if you want to actually do a profound analysis, is that by default it uses GH's public access token, so you'll need to add an environment variable when you call it. Othwerwise, to do a shallow an quick analysis you're fine.

In case you want to do it, I'll guest creating a Fine-grained Personal Access Token that expires soon enough, with a representative name and Read Only access to Public Repositories.

```bash
GH_ACCESS_TOKEN="github_pat_11AABCCDDEE13802849209HD09283CDFFF" gitxray -r theredguild/devsecoops -l -v
```

You should see: `GitHub Token Loaded from GH_ACCESS_TOKEN env variable`, this means you're ready to go.

The first time I wanted to play with a tool like this one, was when I spent hours trying to understand what had happened with the xz backdoor, and how much involvement the infiltrator `JiaT75` had in it. The repo is: https://github.com/tukaani-project/xz.

Let's do a basic analysis.

```bash
wanderer@trg ~ $ gitxray -r tukaani-project/xz -l -v
...
Now verifying repository: tukaani-project/xz
Identified 54 contributors..                                                                      
LISTING CONTRIBUTORS (INCLUDING THOSE WITHOUT A GITHUB USER ACCOUNT) AND EXITING..
Larhzu, JiaT75, thesamesam, adrien-n, jrn, hansjans162, kiyolee, mvatsyk-lsg, afb, maan@tuebingen.mpg.de, bjarniig@rhi.hi.is, junghans, gabibguti, jmarrec, meyering@redhat.com, kianmeng, vapier, skosukhin, stoeckmann, jcallen, kilobyte, bluhm, svpv, radarhere, Coeur, mathstuf, bebuch, bensberg@justemail.net, ChanTsune, parheliamm, ivq, DimitriPapadopoulos, emaste, firasuke, hjl-tools, addaleax, iv-m, Jamaika1, jbastian, jiat75@gmail.com, meyering, vaeth@mathematik.uni-wuerzburg.de, nickajacks1, praiskup, proski@gnu.org, RainRat, ryandesign, sebastianas, vnwildman, scop, xry111, yarikoptic, biergaizi, huangqinjin
```

We can see JiaT75 appearing second. Let's add that username as a parameter to the previous command I'm going to skip to some of the most interesting parts I want to show you.

```plaintext
wanderer@trg ~ $ gitxray -r tukaani-project/xz -c JiaT75
....
Now verifying repository: tukaani-project/xz
YOU HAVE SCOPED THIS GITXRAY TO CONTRIBUTORS: ['JiaT75'] - OTHER USERS WON'T BE ANALYZED.
[comments]:   The Issues comment with the most NEGATIVE reactions (66) is available at: https://github.com/tukaani-project/xz/issues/92#issuecomment-2027786734
[branches]:   12 Unprotected Branches: ['build_werror', 'build_werror2', 'mask_cntrl_chars', 'master', 'memavail', 'single_stream_keeps', 'v5.0', 'v5.2', 'v5.4', 'v5.6', 'version_txt', 
[releases]:   User JiaT75 created historically 10 releases [71.43%] and uploaded a total of 60 assets [66.67%] Turn on Verbose mode for more information.
[contributors]: Top repository contributors with rejected PRs: [stoeckmann 2 rejected out of 2] | [thesamesam 2 rejected out of 2] | [skosukhin 2 rejected out of 2]
[contributors]: Top non-contributor GitHub users with rejected PRs: [brad0 2 rejected out of 2] | [ChenPi12 1 rejected out of 1] | [wzhengsen 1 rejected out of 1]
Some profiling:
[workflows]:  Workflow [CI] created [2022-12-22T13:37:40.000-03:00], updated [2022-12-22T13:37:40.000-03:00]: [https://github.com/tukaani-project/xz/blob/master/.github/workflows/ci.yml]
[workflows]:  Workflow [CI] has [121] missing or deleted runs. This could have been an attacker erasing their tracks, or legitimate cleanup. Set verbose mode for more data. 
[workflows]:  Workflow [CI] was run by [2] NON-contributors [2] times and by [7] contributors [7] times. Set verbose mode for more data. [https://github.com/tukaani-project/xz/actions/workflows/ci.yml]
```

And now specifically about JiaT75

```plaintext
Found results for account JiaT75.
[urls]:       Contributor URL: https://github.com/JiaT75
[urls]:       Owned repositories: https://github.com/JiaT75?tab=repositories
[personal]:   [Name: Jia Tan] obtained from the user's profile.
[emails]:     [jiat0218@gmail.com] obtained from the user's profile.
[emails]:     [jiat0218@gmail.com] obtained by parsing commits.
[profiling]:  Contributor account created: 2021-01-26T18:11:07Z, is 3.73 years old.
[profiling]:  The account was last updated at 2024-03-29T20:19:23Z, 202 days ago.
[profiling]:  Contributor has 10 total public repos.
[profiling]:  Contributor has 646 followers.
[profiling]:  The user submitted 29 Pull Requests out of which 5 were rejected.
[profiling]:  The user submitted 29 Pull Requests out of which 24 were merged.
[commits]:    Made (to this repo) 452 commits, first one at 2022-01-28T12:47:55Z and last one at 2024-03-25T17:50:02Z.
[commits]:    Commit Hours for [452] commits:
            [12pm UTC: 116 (25.66%)]
            [3pm UTC: 73 (16.15%)]
            [1pm UTC: 70 (15.49%)]
            [2pm UTC: 57 (12.61%)]
            [4pm UTC: 52 (11.50%)]
            [5pm UTC: 18 (3.98%)]
            [11am UTC: 16 (3.54%)]
            [8am UTC: 13 (2.88%)]
            [10am UTC: 7 (1.55%)]
            [2am UTC: 6 (1.33%)]
            [3am UTC: 5 (1.11%)]
            [1am UTC: 4 (0.88%)]
            [4am UTC: 4 (0.88%)]
            [7am UTC: 3 (0.66%)]
            [9am UTC: 3 (0.66%)]
            [5am UTC: 1 (0.22%)]
            [7pm UTC: 1 (0.22%)]
            [6am UTC: 1 (0.22%)]
            [12am UTC: 1 (0.22%)]
            [6pm UTC: 1 (0.22%)]
```

As you can see, with only some basic parameters we have a lot of data from which we can infer more information to our research. Like for example, where that person might live if their commits were pushed during the workday, or maybe after work.
