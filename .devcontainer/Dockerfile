FROM python:3.13

ARG USERNAME=vscode
ARG USER_UID=5000
ARG USER_GID=$USER_UID

# Add user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && apt-get clean

# Update package lists
RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get install -y \
    curl \
    git \
    vim \
    less \
    gcc && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Setup git branch in console
RUN echo "source /usr/share/bash-completion/completions/git" >> /home/${USERNAME}/.bashrc

WORKDIR /usr/share/bash-completion/completions

RUN curl -O https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
RUN curl -O https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash

RUN chmod a+x git*.*

RUN ls -l $PWD/git*.* | awk '{print "source "$9}' >> /home/${USERNAME}/.bashrc

RUN echo "GIT_PS1_SHOWDIRTYSTATE=true" >> /home/${USERNAME}/.bashrc
RUN echo "GIT_PS1_SHOWUNTRACKEDFILES=true" >> /home/${USERNAME}/.bashrc
RUN echo "GIT_PS1_SHOWUPSTREAM=auto" >> /home/${USERNAME}/.bashrc
RUN echo 'export PS1="\[\033[01;32m\]\u@\h\[\033[01;33m\] \w \[\033[01;31m\]\$(__git_ps1 \"(%s)\") \\n\[\033[01;34m\]\\$ \[\033[00m\]"' >> /home/${USERNAME}/.bashrc

USER $USERNAME
WORKDIR /workspace

# Install libraries
RUN pip install --no-cache-dir --upgrade pip

CMD ["/bin/bash"]
