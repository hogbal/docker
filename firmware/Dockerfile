FROM ubuntu:22.04
MAINTAINER hogbal31@gmail.com

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install net-tools vim zsh fonts-powerline language-pack-en gdb tmux file dh-autoreconf curl gcc ruby-dev zsh wget -y && \
	apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential -y && \
	apt-get install qemu-system qemu-utils gdb-multiarch binwalk openjdk-18-jdk -y && \
	apt-get install busybox-static fakeroot git dmsetup kpartx netcat-openbsd nmap python3-psycopg2 snmp uml-utilities util-linux vlan sudo -y && \
	gem install seccomp-tools && \
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

WORKDIR /root

CMD ["/usr/bin/zsh"]
