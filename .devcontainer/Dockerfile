FROM ubuntu:24.04

# Set environment variables
ENV ZSH_CUSTOM=/root/.oh-my-zsh/custom
ENV PATH=$PATH:/usr/local/go/bin
ENV GOPATH=/workspace/
ENV GO111MODULE=on
# Install required packages
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    wget \
    gcc \
    make \
    build-essential \
    libssl-dev \
    libffi-dev \
    python3-dev \
    python3-pip \
    python3-venv \
    rsync \
    zsh \
    vim \
    hub \
    yq \
    kubectx \
    autojump \
    && apt-get clean

# set zsh as default shell
RUN chsh -s $(which zsh)

# install on my zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" 
# install zsh theme
RUN git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k 
# install zsh plugins
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting && git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions && git clone https://github.com/rupa/z.git $ZSH_CUSTOM/plugins/z &&   git clone https://github.com/zsh-users/zsh-completions $ZSH_CUSTOM/plugins/zsh-completions 

COPY .zshrc /root/.zshrc 
COPY .p10k.zsh /root/.p10k.zsh

# link python3 to python
RUN ln -s /usr/bin/python3 /usr/bin/python

#  install aws cli
RUN curl -O 'https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip' && unzip awscli-exe-linux-aarch64.zip && ./aws/install && rm -rf awscli-exe-linux-aarch64.zip

# Install go 1.22.5
RUN curl -O https://dl.google.com/go/go1.22.5.linux-arm64.tar.gz
RUN tar -C /usr/local -xzf go1.22.5.linux-arm64.tar.gz
RUN rm go1.22.5.linux-arm64.tar.gz

# install saml2aws 
RUN CURRENT_VERSION=$(curl -Ls https://api.github.com/repos/Versent/saml2aws/releases/latest | grep 'tag_name' | cut -d'v' -f2 | cut -d'"' -f1) && wget https://github.com/Versent/saml2aws/releases/download/v${CURRENT_VERSION}/saml2aws_${CURRENT_VERSION}_linux_arm64.tar.gz && tar -xzvf saml2aws_${CURRENT_VERSION}_linux_arm64.tar.gz && mv saml2aws /usr/local/bin/ && chmod u+x /usr/local/bin/saml2aws

# install kubectl
RUN curl -LO "https://dl.k8s.io/release/v1.22.0/bin/linux/arm64/kubectl" && chmod +x kubectl && mv kubectl /usr/local/bin/

# git config
# RUN git config --global user.email "me@shichanson.vip" && git config --global user.name "shichanson"

# nodejs
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && apt-get install -y nodejs


