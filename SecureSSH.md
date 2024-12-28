# Securing SSH on Linux Server

Securing SSH is essential to protect your server from unauthorized access and brute-force attacks. This guide outlines the steps to secure SSH effectively.

---

## Why Do We Need to Secure SSH?

SSH (Secure Shell) is often the primary method of remote access to servers. If left unsecured, it becomes an attractive target for attackers. Securing SSH helps:

1. **Prevent Unauthorized Access:**
   - By default, SSH listens on port 22, which is widely known and frequently scanned by attackers.
   - Weak passwords or default configurations can be exploited to gain access to the server.

2. **Mitigate Brute-Force Attacks:**
   - Attackers often use automated tools to try thousands of username-password combinations to break into a server.

3. **Protect Sensitive Data:**
   - SSH provides encrypted communication, but without proper security measures, an attacker could still intercept or gain unauthorized access to the session.

4. **Minimize Attack Surface:**
   - Keeping SSH secure ensures that only authorized users and methods are allowed to access the server.

---

## 1. Disable Root Login
Prevent direct root login to enhance security.

**Command:**
```bash
sudo nano /etc/ssh/sshd_config
```

Find the line:
```
PermitRootLogin yes
```

Change it to:
```
PermitRootLogin no
```

Restart the SSH service:
```bash
sudo systemctl restart sshd
```

---

## 2. Change the Default SSH Port
Change the default SSH port (22) to a non-standard port to avoid automated scans.

**Command:**
```bash
sudo nano /etc/ssh/sshd_config
```

Find the line:
```
#Port 22
```

Change it to:
```
Port 2222
```

Save the file and restart the SSH service:
```bash
sudo systemctl restart sshd
```

**Note:** Update your firewall to allow the new port:
```bash
sudo ufw allow 2222/tcp
```

---

## 3. Use SSH Key Authentication
SSH keys provide a secure alternative to password-based login.

**Generate an SSH Key Pair:**
```bash
ssh-keygen -t rsa -b 4096
```

**Copy the Public Key to the Server:**
```bash
ssh-copy-id user@server_ip
```

**Disable Password Authentication:**
```bash
sudo nano /etc/ssh/sshd_config
```

Find the line:
```
PasswordAuthentication yes
```

Change it to:
```
PasswordAuthentication no
```

Restart the SSH service:
```bash
sudo systemctl restart sshd
```

---

## 4. Limit SSH Access to Specific Users
Restrict SSH access to specific users for added security.

**Command:**
```bash
sudo nano /etc/ssh/sshd_config
```

Add the following line:
```
AllowUsers username
```
Replace `username` with the desired user's name.

---

## 5. Enable a Firewall
Use a firewall to control access to the SSH port.

**For UFW:**
```bash
sudo ufw allow 2222/tcp
sudo ufw enable
```

---

## 6. Use Fail2Ban
Install Fail2Ban to block IPs showing signs of malicious activity.

**Install Fail2Ban:**
```bash
sudo apt install fail2ban
```

**Start the Service:**
```bash
sudo systemctl enable --now fail2ban
```

---

## 7. Log and Monitor SSH Activity
Regularly check SSH logs for suspicious activity.

**Command:**
```bash
sudo tail -f /var/log/auth.log
```

---

## Summary of Commands

| Task                            | Command/Action                                          |
|---------------------------------|--------------------------------------------------------|
| Disable root login              | `PermitRootLogin no` in `/etc/ssh/sshd_config`         |
| Change default SSH port         | Change `Port` in `/etc/ssh/sshd_config`               |
| Use SSH key authentication      | Generate and use SSH keys                             |
| Disable password authentication | `PasswordAuthentication no` in `/etc/ssh/sshd_config` |
| Limit SSH access to users       | `AllowUsers username` in `/etc/ssh/sshd_config`       |
| Enable a firewall               | Use `ufw` or `iptables` to allow SSH port             |
| Use Fail2Ban                    | Install and configure Fail2Ban                        |
| Monitor SSH logs                | `sudo tail -f /var/log/auth.log`                      |

---

By implementing these steps, you can significantly improve the security of your SSH service.
