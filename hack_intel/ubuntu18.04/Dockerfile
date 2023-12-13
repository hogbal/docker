FROM ubuntu:18.04
MAINTAINER hogbal31@gmail.com

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install net-tools openssh-server vim ruby zsh fonts-powerline language-pack-en gdb tmux file gdbserver dh-autoreconf curl gcc ruby-dev -y && \
	apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential -y && \
	pip3 install ropgadget && \
	update-locale

# x86
RUN dpkg --add-architecture i386 && \
	apt-get update && \
	apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 -y

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
	sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' /root/.zshrc
RUN chsh -s /usr/bin/zsh | echo 'root'
RUN git clone https://github.com/hogbal/hogbal.zsh-theme.git
RUN cp /root/tools/hogbal.zsh-theme/hogbal.zsh-theme /root/.oh-my-zsh/themes/hogbal.zsh-theme
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="hogbal"/' /root/.zshrc
ENV REMOTE_HOST="x86_64 ubuntu18.04"

#install pwntools
WORKDIR /root
RUN python3 -m pip install --upgrade pip && \
    python3 -m pip install --upgrade pwntools

#install checksec
WORKDIR /root/tools
RUN git clone https://github.com/slimm609/checksec.sh.git
RUN cp checksec.sh/checksec /usr/local/bin

#set gdb
COPY gdbinit /root/.gdbinit
RUN mkdir /root/tools/gdb

#install gef
WORKDIR /root/tools/gdb
RUN git clone https://github.com/hugsy/gef.git
RUN echo "source /root/tools/gdb/gef/gef.py" > /root/.gdbinit_gef

RUN echo "export LC_ALL=en_US.UTF-8" >> /root/.zshrc
RUN echo "export PYTHONIOENCODING=UTF-8" >> /root/.zshrc

WORKDIR /root

CMD ["/usr/bin/zsh"]
