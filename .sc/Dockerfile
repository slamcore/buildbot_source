FROM ubuntu:22.04

# Arguments --------------------------------------------------------------------
ARG USERNAME=slamcore
ARG UID=1000
ARG GID=1000

# Install python and buildbot --------------------------------------------------

RUN \
        apt-get update && \
        apt-get --assume-yes install \
          build-essential \
          git \
          lsb-release \
          bash \
          curl \
          python3 \
          python3-pip \
          python3-virtualenv \
          software-properties-common \
          sudo \
          wget

# Install node

RUN \
        apt-get update && \
        apt-get --assume-yes install \
          apt-transport-https \
          ca-certificates \
          gnupg && \
        mkdir -p /usr/share/keyrings && \
        (rm -f /usr/share/keyrings/nodesource.gpg || true) && \
        (rm -f /etc/apt/sources.list.d/nodesource.list || true) && \
        (curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /usr/share/keyrings/nodesource.gpg) && \
        chmod 644 /usr/share/keyrings/nodesource.gpg && \
        echo "deb [arch=amd64 signed-by=/usr/share/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list && \
        echo "Package: nsolid" | tee /etc/apt/preferences.d/nsolid && \
        echo "Pin: origin deb.nodesource.com" | tee -a /etc/apt/preferences.d/nsolid && \
        echo "Pin-Priority: 600" | tee -a /etc/apt/preferences.d/nsolid && \
        echo "Package: nodejs" | tee /etc/apt/preferences.d/nodejs && \
        echo "Pin: origin deb.nodesource.com" | tee -a /etc/apt/preferences.d/nodejs && \
        echo "Pin-Priority: 600" | tee -a /etc/apt/preferences.d/nodejs && \
        apt-get update && \
        apt-get install -y "nodejs=22.15.1-1nodesource1"

# Install yarn

RUN \
        curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null && \
        echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && \
        sudo apt-get update && \
        sudo apt-get install "yarn=1.22.22-1"

# create user ------------------------------------------------------------------
RUN groupadd -g $GID -o $USERNAME                                              \
  && useradd -m -u $UID -g $GID --groups sudo -p $(openssl passwd -1 $USERNAME) $USERNAME \
  && usermod -aG sudo $USERNAME                                                \
  && echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USERNAME       \
  && cat /etc/sudoers.d/$USERNAME

USER $USERNAME
