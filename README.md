# docker
Dockerfile for solving CTF Pwnable problems.  

I made it exclusively for `arm64` architecture and `x86_64` architecture. 

The intel architecture has installed a package that can also decode 32-bit binaries.

Please refer to the link below for the docker image that has already been built.

- [hack_intel](https://hub.docker.com/repository/docker/hogbal/hack_intel/general)
- [hack_arm](https://hub.docker.com/repository/docker/hogbal/hack_arm/general)

# Usage

```
docker run -it --rm --cap-add SYS_PTRACE --security-opt seccomp:unconfined -v <local_path>:<docker_path> <docker image>
```

# Installed

- gef
- [oh-my-zsh](https://github.com/hogbal/hogbal.zsh-theme)
- vim
- pwntools
- one_gadget
- checksec
- patchelf
