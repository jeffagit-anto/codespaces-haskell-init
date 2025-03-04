ARG VARIANT="bookworm-slim"
FROM debian:${VARIANT}

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y build-essential curl libffi-dev libffi8 libgmp-dev libgmp10 libncurses-dev libncurses5 libtinfo5 sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set vscode as the default user.
USER $USERNAME

# Install ghcup in non interactive mode
RUN sudo curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | BOOTSTRAP_HASKELL_NONINTERACTIVE=1 sh

ENV PATH="/home/vscode/.cabal/bin:/home/vscode/.ghcup/bin:$PATH"

ENTRYPOINT ghc -e '"Hello Haskell!"'