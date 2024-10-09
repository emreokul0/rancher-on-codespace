@emreokul0 ➜ /workspaces/rancher-on-codespace (main) $ sudo apt-get install docker-ce docker-ce-cli containerd.io
Reading package lists... Done
Building dependency tree       
Reading state information... Done
docker-ce-cli is already the newest version (5:27.3.1-1~ubuntu.20.04~focal).
docker-ce-cli set to manually installed.
The following package was automatically installed and is no longer required:
  moby-tini
Use 'sudo apt autoremove' to remove it.
Suggested packages:
  aufs-tools cgroupfs-mount | cgroup-lite
The following NEW packages will be installed:
  docker-ce
The following packages will be upgraded:
  containerd.io
1 upgraded, 1 newly installed, 0 to remove and 10 not upgraded.
Need to get 55.1 MB of archives.
After this operation, 115 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 https://download.docker.com/linux/ubuntu focal/stable amd64 containerd.io amd64 1.7.22-1 [29.5 MB]
Get:2 https://download.docker.com/linux/ubuntu focal/stable amd64 docker-ce amd64 5:27.3.1-1~ubuntu.20.04~focal [25.6 MB]
Fetched 55.1 MB in 1s (82.9 MB/s)
(Reading database ... 70436 files and directories currently installed.)
Preparing to unpack .../containerd.io_1.7.22-1_amd64.deb ...
Unpacking containerd.io (1.7.22-1) over (1.6.24-1) ...
Preparing to unpack .../docker-ce_5%3a27.3.1-1~ubuntu.20.04~focal_amd64.deb ...
Unpacking docker-ce (5:27.3.1-1~ubuntu.20.04~focal) ...
dpkg: error processing archive /var/cache/apt/archives/docker-ce_5%3a27.3.1-1~ubuntu.20.04~focal_amd64.deb (--unpack):
 trying to overwrite '/usr/libexec/docker/docker-init', which is also in package moby-tini 0.19.0-ubuntu20.04u1
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/docker-ce_5%3a27.3.1-1~ubuntu.20.04~focal_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)

--------------------------------------------------------

The error you're encountering indicates a conflict between Docker's `docker-init` and the `moby-tini` package. Specifically, both packages attempt to install a file at `/usr/libexec/docker/docker-init`, which is causing a conflict during the installation of `docker-ce`.

### Steps to Resolve the Conflict

#### Step 1: **Remove the Conflicting Package (`moby-tini`)**
The package `moby-tini` is no longer required, as indicated in the error message. You can remove it to resolve the conflict.

```bash
sudo apt-get remove moby-tini
```

This should remove `moby-tini`, which is conflicting with the installation of Docker.

#### Step 2: **Clean Up Any Leftover Packages**
Run the following command to clean up any partially installed packages and ensure your package list is up-to-date:

```bash
sudo apt-get autoremove
sudo apt-get clean
sudo apt-get update
```

#### Step 3: **Reinstall Docker**
Now, try reinstalling Docker after removing the conflicting package:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

#### Step 4: **Verify Docker Installation**
Once the installation completes successfully, verify that Docker is installed:

```bash
docker --version
```

This should return the installed Docker version.

#### Optional: **Force Overwrite (Not Recommended)**

If for some reason you cannot remove `moby-tini` (or it's critical for your setup), you could force `dpkg` to overwrite the conflicting file. However, this is not recommended because it can lead to unstable behavior:

```bash
sudo dpkg -i --force-overwrite /var/cache/apt/archives/docker-ce_5%3a27.3.1-1~ubuntu.20.04~focal_amd64.deb
```

This forces the installation to overwrite the existing `docker-init` file, but it may cause issues if `moby-tini` is used in your environment.

### Conclusion
The best course of action is to remove `moby-tini` to resolve the conflict and allow Docker to install cleanly. After the steps above, Docker should install successfully. Let me know if you encounter further issues!