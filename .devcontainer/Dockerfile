ARG VARIANT="slim"
FROM haskell:${VARIANT}

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo gnupg \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

COPY ./get-ghcup.haskell.org.sh /tmp/get-ghcup.haskell.org.sh

RUN gpg --batch --keyserver keyserver.ubuntu.com --recv-keys 7D1E8AFD1D4A16D71FADA2F2CCC85C0E40C06A8C \
    && gpg --batch --keyserver keyserver.ubuntu.com --recv-keys FE5AB6C91FEA597C3B31180B73EDE9E8CFBAEF01 \
    && gpg --batch --keyserver keyserver.ubuntu.com --recv-keys 88B57FCF7DB53B4DB3BFA4B1588764FBE22D19C4 \
    && gpg --batch --keyserver keyserver.ubuntu.com --recv-keys EAF2A9A722C0C96F2B431CA511AAD8CEDEE0CAEF

# Set vscode as the default user.
USER $USERNAME

# Install ghcup minimal+ on interactive, see 'get-ghcup.haskell.org.sh' file
RUN sudo sh /tmp/get-ghcup.haskell.org.sh

# RUN export PATH="/root/.ghcup/bin:$PATH"

# Install HLS, with a special glimpse for 9.10.1
# see https://github.com/haskell/haskell-language-server/issues/3225#issuecomment-2660166509
# RUN sudo /root/.ghcup/bin/ghcup compile hls --git-ref master --ghc 9.10.1 --cabal-update
# RUN sudo /root/.ghcup/bin/ghcup compile hls -v 2.9.0.1 --ghc 9.10.1 --cabal-update
# RUN sudo /root/.ghcup/bin/ghcup compile hls --help

# Process "cabal" with arguments ["v2-install","-w","ghc-9.10.1","--install-method=copy","--overwrite-policy=always","--disable-profiling","--disable-tests","--installdir=/root/.ghcup/tmp/ghcup-36c14808a9b6ce2e/out/9.10.1","--project-file=cabal.project","exe:haskell-language-server","exe:haskell-language-server-wrapper"] failed with exit code 1.
# rm -rf /tmp/get-ghcup.haskell.org.sh
