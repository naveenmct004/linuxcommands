## 1. Check Current Hostname

```bash
hostnamectl
```
**Explanation:** Displays the current hostname and system information, including the static hostname, machine ID, and more.

Alternative command:

```bash
hostname
```
**Explanation:** Shows only the current hostname.

---

## 2. Set a New Hostname

```bash
sudo hostnamectl set-hostname new-hostname
```
**Explanation:** Changes the hostname to `new-hostname`. Replace `new-hostname` with your desired hostname.

Example:

```bash
sudo hostnamectl set-hostname myserver
```

---

## 3. Edit `/etc/hostname` File (Optional)

```bash
sudo nano /etc/hostname
```
**Explanation:** Opens the `/etc/hostname` file in the Nano editor. Replace the existing hostname with your new desired hostname. Save and exit.

---

## 4. Edit `/etc/hosts` File

```bash
sudo nano /etc/hosts
```
**Explanation:** Updates the `/etc/hosts` file to reflect the new hostname. Find the line that looks like this:

```
127.0.1.1   old-hostname
```

Change `old-hostname` to your new hostname:

```
127.0.1.1   myserver
```

Save and exit.

---

## 5. Reboot the System

```bash
sudo reboot
```
**Explanation:** Reboots the system to apply hostname changes.

Alternatively, restart the hostname service without rebooting:

```bash
sudo systemctl restart systemd-hostnamed
```
**Explanation:** Restarts the hostname service to apply changes immediately.

---

## 6. Verify the Hostname Change

```bash
hostnamectl
```
**Explanation:** Displays the updated hostname and system information.

Alternative command:

```bash
hostname
```
**Explanation:** Shows only the current hostname.

---

## 7. Deactivate or Reset the Hostname

```bash
sudo hostnamectl set-hostname ""
```
**Explanation:** Removes the hostname. The system will revert to a default or generic hostname.

---

## 8. List All Hostnames

```bash
hostnamectl list
```
**Explanation:** Lists all hostnames set on the system (if supported by your Linux distribution).

---

## Summary of Commands

| Task                           | Command                                      |
|--------------------------------|----------------------------------------------|
| Check current hostname         | `hostnamectl` or `hostname`                 |
| Set a new hostname             | `sudo hostnamectl set-hostname new-hostname`|
| Edit `/etc/hostname`           | `sudo nano /etc/hostname`                   |
| Edit `/etc/hosts`              | `sudo nano /etc/hosts`                      |
| Reboot the system              | `sudo reboot`                               |
| Restart hostname service       | `sudo systemctl restart systemd-hostnamed`  |
| Verify hostname                | `hostnamectl` or `hostname`                 |
| Reset hostname                 | `sudo hostnamectl set-hostname ""`         |
| List all hostnames             | `hostnamectl list`                          |
