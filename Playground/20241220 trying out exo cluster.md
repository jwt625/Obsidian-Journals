
Installation very smooth:
![[Pasted image 20241220153052.png]]

![[Pasted image 20241220153034.png]]


2024-12-20T15:29:02-08:00
Ran into issue the download button not working.
Seems fixed after running the python install certificates:
![[Pasted image 20241220153001.png]]

Downloading:


![[Pasted image 20241220152903.png]]


LLaVa is not supported it seems...
Going to try Llama 3.3 70B. Might not have enough memory lol. Let's see.

![[Pasted image 20241220160551.png]]

# On ubuntu
2024-12-23T11:24:12-08:00
Installed on 2020 desktop's ubuntu 20.04.
Basically following CUDA instructions with a lot of help from claude. As well as restart after the isntallation:
https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

Tests:
`nvidia-smi`:
![[Pasted image 20241223113009.png]]

`deviceQuery`:
![[Pasted image 20241223113109.png]]

`bandwidthTest`:
![[Pasted image 20241223113137.png]]


`nvcc --version`:
![[Pasted image 20241223114457.png]]


## Installing cuDNN library
2024-12-23T11:45:54-08:00

https://developer.nvidia.com/cudnn-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local


```
wget https://developer.download.nvidia.com/compute/cudnn/9.6.0/local_installers/cudnn-local-repo-ubuntu2004-9.6.0_1.0-1_amd64.debsudo dpkg -i cudnn-local-repo-ubuntu2004-9.6.0_1.0-1_amd64.debsudo cp /var/cudnn-local-repo-ubuntu2004-9.6.0/cudnn-*-keyring.gpg /usr/share/keyrings/sudo apt-get updatesudo apt-get -y install cudnn
```


Verification:
- https://docs.nvidia.com/deeplearning/cudnn/latest/installation/linux.html#verifying-the-install-on-linux
- need some debugging with claude
- `./mnistCUDNN` test passed:
![[Pasted image 20241223115556.png]]





