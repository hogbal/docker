FROM ubuntu:22.04
MAINTAINER hogbal31@gmail.com

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install net-tools openssh-server vim ruby zsh fonts-powerline language-pack-en gdb tmux file gdbserver dh-autoreconf curl gcc ruby-dev cpio -y && \
	apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential gcc-x86-64-linux-gnu -y && \
	apt-get install vim git make gcc build-essential rsync fakeroot libncurses-dev libncurses5 libncurses5-dev bin86 libssl-dev libelf-dev xz-utils curl wget bc flex bison kmod -y && \
	pip3 install ropgadget && \
	update-locale

WORKDIR /root
RUN mkdir /root/tools

#install vim
COPY vimrc /root/.vimrc

#install oh my zsh
WORKDIR /root/tools
RUN sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)" && \
	git clone https://github.com/zsh-users/zsh-syntax-highlighting.git /root/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && \
	git clone https://github.com/zsh-users/zsh-autosuggestions  /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
	sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' /root/.zshrc
RUN chsh -s /usr/bin/zsh | echo 'root'
RUN git clone https://github.com/hogbal/hogbal.zsh-theme.git
RUN cp /root/tools/hogbal.zsh-theme/hogbal.zsh-theme /root/.oh-my-zsh/themes/hogbal.zsh-theme
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="hogbal"/' /root/.zshrc
WORKDIR /root

#setting ssh
EXPOSE 22
RUN mkdir -p /run/sshd && chmod 755 /run/sshd
RUN echo 'root:root' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'export REMOTE_HOST="kernel22.04"' >> /root/.zshrc
RUN git config --global oh-my-zsh.hide-status 1
RUN git config --global oh-my-zsh.hide-dirty 1

CMD /usr/sbin/sshd -D
