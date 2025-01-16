# Installing Docker Compose on Linux Server

## 1. Install Docker (if not already installed)

Ensure Docker is installed on your Linux server:

```bash
sudo apt update
sudo apt install docker.io -y
```

Enable and start the Docker service:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Verify the Docker installation:

```bash
docker --version
```

---

## 2. Install Docker Compose

### Option A: Using the Latest Binary

1. Download the Docker Compose binary:
   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. Apply executable permissions:
   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. Verify the installation:
   ```bash
   docker-compose --version
   ```

### Option B: Install via `apt` (Debian/Ubuntu)

1. Update the `docker-compose-plugin` package (requires Docker CLI):
   ```bash
   sudo apt update
   sudo apt install docker-compose-plugin
   ```

2. Verify the plugin version:
   ```bash
   docker compose version
   ```

---

## 3. Configure Permissions

If you want to use Docker without `sudo`, add your user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Log out and log back in to apply the changes.

---

## 4. Test Docker Compose

1. Create a sample `docker-compose.yml` file:
   ```yaml
   version: '3'
   services:
     hello-world:
       image: hello-world
   ```

2. Run Docker Compose:
   ```bash
   docker-compose up
   ```

---

Your Docker Compose should now be installed and ready for use!
