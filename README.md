# archipelago-server-docker
Docker setup for archipelago server


# Setup

1. Get the same Zelda ROM needed in this [guide](https://archipelago.gg/tutorial/A%20Link%20to%20the%20Past/multiworld/en), rename it to "Zelda no Densetsu - Kamigami no Triforce (Japan).sfc" and place it in the build folder.
3. Copy config.yaml.default to config.yaml and configure archipelago.
4. Create folders: `db`, `logs`, `uploads`
5. Run `docker compose up --build -d`

# MultiWorldServer Ports
Ports are choosen randomly from [49152 to 65535](https://github.com/ArchipelagoMW/Archipelago/blob/main/WebHostLib/customserver.py#L164).
