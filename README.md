# docker
A repository with Dockerfile installed to help solve CTF pwnable problems and binwalk to help extract firmware.

The `firmware` folder has Dockerfile for extracting firmware. 

And the `hack_intel` and `hack_amd` folders have Dockerfile that operates on the "x86_64" architecture and the "Amd64" architecture, respectively.

Please refer to the link below for the docker image that has already been built.

- [hack_intel](https://hub.docker.com/repository/docker/hogbal/hack_intel/general)
- [hack_arm](https://hub.docker.com/repository/docker/hogbal/hack_arm/general)

# Usage

```
docker run -it --rm --cap-add SYS_PTRACE --security-opt seccomp:unconfined -v <local_path>:<docker_path> <docker image>
```

# Installed
The tools installed in the hack_intel and hack_amd folders are as follows.

- gef
- [oh-my-zsh](https://github.com/hogbal/hogbal.zsh-theme)
- vim
- pwntools
- one_gadget
- checksec
- patchelf
