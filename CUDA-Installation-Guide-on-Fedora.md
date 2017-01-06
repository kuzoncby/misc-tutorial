# CUDA Installation Guide on Fedora
Go to [_CUDA Toolkit_](https://developer.nvidia.com/cuda-downloads) download page. My system is fedora, so I go **Linux > x86_64 > Fedora > 23 > rpm(network)**.

Then there will be a panel show up. Click download button and you will get a rpm file.

Next:
* rpm -i cuda-repo-fedora23-8.0.44-1.x86_64.rpm
* dnf install cuda

And you got cuda on your device.

#### Add to PATH
Edit your **~/.bashrc** file. Add those:

```sh
export PATH=$PATH:/usr/local/cuda/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
```

Then, make it work:
```bash
$ source  ~/.bashrc
```
