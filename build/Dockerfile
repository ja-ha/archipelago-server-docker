FROM debian:bookworm-slim

ARG VERSION=0.4.6
ARG USERNAME=user-name-goes-here
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y tar python3 python3-pip python3-venv python3-tk wget git tini && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

# setup uploads volume
RUN mkdir /uploads && chown -R $USER_UID:$USER_GID /uploads && mkdir /db && chown -R $USER_UID:$USER_GID /db
VOLUME /uploads
VOLUME /db

# dl archipelago
RUN cd /tmp && wget https://github.com/ArchipelagoMW/Archipelago/archive/refs/tags/$VERSION.tar.gz -O archipelago.tar.gz && tar xf archipelago.tar.gz && \
    mv /tmp/Archipelago-$VERSION/ /opt/archipelago && rm archipelago.tar.gz && \
    chown -R $USER_UID:$USER_GID /opt/archipelago

# change user
USER $USERNAME

WORKDIR /opt/archipelago

COPY --chown=$USER_UID:$USER_GID ["Zelda no Densetsu - Kamigami no Triforce (Japan).sfc", "/opt/archipelago"]

# install python packages
RUN python3 -m venv --system-site-packages --upgrade-deps /opt/archipelago/venv && \
    /opt/archipelago/venv/bin/pip install --no-cache-dir -r WebHostLib/requirements.txt
RUN yes "" | /opt/archipelago/venv/bin/python -c "import WebHost;from WebHostLib.lttpsprites import update_sprites_lttp;update_sprites_lttp()"

# run server
ENTRYPOINT ["tini", "--"]
CMD ["/opt/archipelago/venv/bin/python3", "-u", "WebHost.py"]
