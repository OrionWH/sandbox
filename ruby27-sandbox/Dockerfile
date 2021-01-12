FROM ubuntu:20.04

#-- some environments ------------------------------------------------------------------
ENV RUBY_VERSION=ruby-2.7.2
ENV NVM_VERSION=v0.37.2
ENV NODE_VERSION=v12.13.0
ENV USER=fox

SHELL ["/bin/bash", "-c"]

#-- install some packages --------------------------------------------------------------
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
  vim \
  git \
  curl \
  wget \
  iproute2 \
  sudo \
  gpg \
  tzdata

RUN useradd -s /bin/bash -G sudo -m ${USER}
ADD sudo /etc/sudoers.d/sudo

USER ${USER}
WORKDIR /home/${USER}

#-- install rmv & ruby -----------------------------------------------------------------
#RUN gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
RUN \curl -sSL https://get.rvm.io | bash
RUN source ${HOME}/.rvm/scripts/rvm && rvm install ${RUBY_VERSION}
RUN source ${HOME}/.rvm/scripts/rvm && rvm list

#-- install yarn & node ----------------------------------------------------------------
RUN wget -qO- https://raw.githubusercontent.com/creationix/nvm/${NVM_VERSION}/install.sh | bash
RUN export NVM_DIR="$HOME/.nvm" && \. $NVM_DIR/nvm.sh && nvm install ${NODE_VERSION}

#-- make bash prompt nice --------------------------------------------------------------
RUN echo 'parse_git_branch() {' >> ${HOME}/.bashrc
RUN echo "  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'" >> ${HOME}/.bashrc
RUN echo '}'  >> ${HOME}/.bashrc
RUN echo 'export PS1="\[\033[01;35m\]\u@\h \[\033[01;34m\]\w\[\033[91m\]\$(parse_git_branch)\[\033[00m\] $ "' >> ${HOME}/.bashrc

CMD sleep infinity
