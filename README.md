# DevSecOops

Hi! And welcome to the DevSecOops repository. An educational repository complementary to our [@theredguild/devsecops-toolkit](https://github.com/theredguild/devsecops-toolkit) where we provide practical aid on how to use some of the tools depicted there.

![AI Generated](src/intro/oops.png)


**To start,** you should have at least built the `workshop-minimal` image of that container.

```bash
git clone https://github.com/theredguild/devsecops-toolkit
git checkout workshop-minimal
make exec
```

You should be able to see something similar to this.

```plaintext
❯ make exec
fatal: No names found, cannot describe anything.
Running interactive shell inside the devsecops-toolkit container...
                    __        __   _                               
                    \ \      / /__| | ___ ___  _ __ ___   ___      
                     \ \ /\ / / _ \ |/ __/ _ \| '_ ` _ \ / _ \     
                      \ V  V /  __/ | (_| (_) | | | | | |  __/     
                    __ \_/\_/ \___|_|\___\___/|_| |_| |_|\___|     
                    \ \      / /_ _ _ __   __| | ___ _ __ ___ _ __ 
                     \ \ /\ / / _` | '_ \ / _` |/ _ \ '__/ _ \ '__|
                      \ V  V / (_| | | | | (_| |  __/ | |  __/ |   
                       \_/\_/ \__,_|_| |_|\__,_|\___|_|  \___|_|   

                         Welcome to the devsecops toolkit
                            Built by The Red Guild 🪷

    This container was created as a resource for a workshop, 
    which intends to spread awareness, help people protect themselves 
    and the repos they interact with. Say hi @theredguild!, don't be a stranger.

wanderer@trg ~ $ 
```

We don't use these tools daily, but they are a selection and a curation of our own after thorough research, testing, and asking around :). If you want more tools, you can checkout `develop` but today in its current state it weights like +10GB.

## Handbook

If you haven't noticed, we migrated the practical content from the README.md to a book. So go check it out! DevSecOops.theredguild.org