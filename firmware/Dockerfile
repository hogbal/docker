FROM ubuntu:22.04
MAINTAINER hogbal31@gmail.com

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install net-tools vim zsh fonts-powerline language-pack-en gdb tmux file dh-autoreconf curl gcc ruby-dev zsh wget -y && \
	apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential -y && \
	apt-get install qemu-system qemu-user-static qemu-utils gdb-multiarch openjdk-18-jdk -y && \
	apt-get install binwalk busybox-static fakeroot git dmsetup kpartx netcat-openbsd nmap python3-psycopg2 snmp uml-utilities util-linux vlan sudo -y && \
	apt-get install gcc gcc-aarch64-linux-gnu -y && \
	apt-get install bc bison build-essential coccinelle \
  					device-tree-compiler dfu-util efitools flex gdisk graphviz imagemagick \
  					liblz4-tool libgnutls28-dev libguestfs-tools libncurses-dev \
  					libpython3-dev libsdl2-dev libssl-dev lz4 lzma lzma-alone openssl \
  					pkg-config python3 python3-asteval python3-coverage python3-filelock \
  					python3-pkg-resources python3-pycryptodome python3-pyelftools \
  					python3-pytest python3-pytest-xdist python3-sphinxcontrib.apidoc \
  					python3-sphinx-rtd-theme python3-subunit python3-testtools \
  					python3-virtualenv swig uuid-dev -y && \
	apt-get install zlib1g-dev liblzma-dev liblzo2-dev liblzo2-dev -y && \
	gem install seccomp-tools && \
	pip3 install nose coverage pycryptodome pyqtgraph capstone cstruct && \
	update-locale

WORKDIR /root
RUN mkdir /root/tools

#install vim
COPY vimrc /root/.vimrc

#install tmux
WORKDIR /root
RUN git clone https://github.com/gpakosz/.tmux.git
RUN ln -s -f .tmux/.tmux.conf
RUN cp .tmux/.tmux.conf.local .

#install oh my zsh
WORKDIR /root/tools
RUN sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)" && \
	git clone https://github.com/zsh-users/zsh-syntax-highlighting.git /root/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && \
	git clone https://github.com/zsh-users/zsh-autosuggestions  /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
	sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' /root/.zshrc && \
	sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/' /root/.zshrc
RUN chsh -s /usr/bin/zsh | echo 'root'

#set gdb
COPY gdbinit /root/.gdbinit
RUN mkdir /root/tools/gdb

#install pwndbg
WORKDIR /root/tools/gdb
RUN git clone https://github.com/pwndbg/pwndbg
WORKDIR pwndbg
RUN ./setup.sh
RUN echo "source /root/tools/gdb/pwndbg/gdbinit.py" > /root/.gdbinit_pwndbg

#install gef
WORKDIR /root/tools/gdb
RUN git clone https://github.com/hugsy/gef.git
RUN echo "source /root/tools/gdb/gef/gef.py" > /root/.gdbinit_gef

RUN echo "export LC_ALL=en_US.UTF-8" >> /root/.zshrc
RUN echo "export PYTHONIOENCODING=UTF-8" >> /root/.zshrc

RUN echo '#!/bin/sh\nexec gdb -q -ex init-pwndbg "$@"' > /usr/bin/gdb-pwndbg
RUN echo '#!/bin/sh\nexec gdb -q -ex init-gef "$@"' > /usr/bin/gdb-gef
RUN chmod +x /usr/bin/gdb-*

#install fmk
WORKDIR /root/tools
RUN git clone https://github.com/rampageX/firmware-mod-kit.git

WORKDIR /root

CMD ["/usr/bin/zsh"]
