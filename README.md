# Minecraft Craft to Exile 2 Server Docker Compose Setup

This Docker Compose configuration sets up a **Minecraft server** running the **Craft to Exile 2** modpack using the `itzg/minecraft-server` image. It includes:

- Persistent data storage for your world, mods, and configurations.
- Optimized JVM settings for performance with 6GB of allocated memory.
- CurseForge integration to automatically download and sync the Craft to Exile 2 modpack.
- RCON for remote server management.
- A bridge network and health checks for reliability.

## Prerequisites

- **Docker** and **Docker Compose** installed on your host machine.
- A **CurseForge API key** (available from the [CurseForge developer portal](https://docs.curseforge.com/#getting-started)).
- The **Craft to Exile 2 server modpack ZIP** file (e.g., `Craft to Exile 2 SERVER-1.0.7.zip`).
- Basic familiarity with editing YAML files and running terminal commands.

## Setup Instructions

1. **Create a Project Directory**  
   Create a directory (e.g., `minecraft-cte2-server`) and place the `docker-compose.yml` file inside it.

2. **Edit the `docker-compose.yml` File**  
   Open `docker-compose.yml` in a text editor and replace the placeholders (in `[ ]`) with your values:  
   - `[25565]`: Host port for Minecraft (default: `25565`).  
   - `[/data/Craft to Exile 2 SERVER-1.0.7.zip]`: Path to your modpack ZIP inside the container (e.g., `/data/Craft to Exile 2 SERVER-1.0.7.zip`).  
   - `[KEY123]`: Your CurseForge API key.  
   - `[CTE2 Server]`: Server name (e.g., `My CTE2 Server`).  
   - `[A craft-to-exile-2 Server.]`: Message of the Day (MOTD) for players.  
   - `"[100]"`: Maximum players (as a string, e.g., `"20"`).  
   - `[!Password!]`: Secure RCON password (use a strong, unique password).  
   - `"[25575]"`: RCON port (as a string, e.g., `"25575"`).  
   - `[/data]`: Host path for data persistence (e.g., `./minecraft-data` for a relative path or `/path/to/your/data` for an absolute path).  
   - *Optional*: Uncomment volume mounts (e.g., `server.properties`, `whitelist.json`, `ops.json`) to manage specific config files from the host.

3. **Prepare the Data Volume**  
   - Create a directory on your host for persistent data (e.g., `./minecraft-data`).  
   - Place the Craft to Exile 2 server modpack ZIP file in this directory. The container will access it via the volume mount.

4. **Start the Server**  
   In your project directory, run:  
   ```bash
   docker compose up -d
   ```  
   This pulls the `itzg/minecraft-server` image, installs the modpack, and starts the container. Initial startup may take a few minutes.

5. **Access the Server**  
   - **Minecraft**: Connect using a Minecraft client at `your-host-ip:25565` (or `localhost:25565` if local).  
   - **RCON**: Use an RCON client (e.g., [mcrcon](https://github.com/Tiiffi/mcrcon)) with `your-host-ip:25575` and your RCON password.

6. **Monitor and Manage**  
   - View logs:  
     ```bash
     docker compose logs -f
     ```  
   - Stop the server:  
     ```bash
     docker compose down
     ```  
   - Restart:  
     ```bash
     docker compose restart
     ```  
   - Check health:  
     ```bash
     docker inspect craft-to-exile-2 --format "{{.State.Health.Status}}"
     ```  
   - Customize: Edit files in your host's data directory (e.g., `server.properties`), then restart the container.

7. **Updates and Maintenance**  
   - **Modpack Update**: Replace the ZIP file in your data directory and restart the container. Ensure `CF_FORCE_SYNCHRONIZE: "true"` is set.  
   - **Image Update**: Run `docker compose pull` then `docker compose up -d`.  
   - **Backups**: Regularly copy the data directory to back up your world and configs.

## Notes

- **Security**: If exposing ports publicly, secure with a firewall or VPN. Keep your RCON password confidential.  
- **Performance**: 6GB RAM is allocated; adjust `MEMORY` based on your host's capacity.  
- **Troubleshooting**: Check logs for errors (e.g., invalid API key or modpack issues).  
- **Further Customization**: Explore additional settings in the [itzg/minecraft-server documentation](https://github.com/itzg/docker-minecraft-server).  

Enjoy your **Craft to Exile 2** adventure!
