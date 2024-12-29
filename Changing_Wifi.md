# How to Connect to a New Wi-Fi Network and Configure a Static IP

This guide explains how to connect to a new Wi-Fi network and optionally configure a static IP address for consistent connectivity.

---

## Why Do We Need a Static IP Address for Wi-Fi?

1. **Consistent Addressing:**
   - Ensures the system always has the same IP address, simplifying remote access and configuration.

2. **Improved Reliability:**
   - Avoids disruptions caused by dynamically assigned IP addresses changing over time.

3. **Critical for Services:**
   - Essential for hosting services, file sharing, or networked applications.

4. **Custom Configuration:**
   - Allows you to tailor settings for specific needs, such as routing or DNS customization.

---

## Step-by-Step Guide

### Step 1: Identify Available Wi-Fi Networks

To see the list of available Wi-Fi networks:
```bash
nmcli dev wifi list
```
This command displays all Wi-Fi networks within range of your device.

---

### Step 2: Connect to the New Wi-Fi Network

Use the following command to connect to a network:
```bash
nmcli dev wifi connect "<SSID>" password "<password>"
```
- Replace `<SSID>` with the Wi-Fi network name.
- Replace `<password>` with the Wi-Fi password.

### Verify the Connection
After connecting, check the status:
```bash
nmcli dev status
```
Ensure your Wi-Fi interface (e.g., `wlan0`) is listed as "connected."

---

## Configuring a Static IP Address for Wi-Fi

If you want to assign a static IP address to your Wi-Fi connection, follow these steps:

### Step 1: Edit the Netplan Configuration File

1. Open the Netplan configuration file:
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```

2. Add or update the Wi-Fi section with the following:

   Example:
   ```yaml
   network:
     version: 2
     renderer: networkd
     wifis:
       wlan0:
         dhcp4: no
         addresses:
           - 192.168.1.150/24
         gateway4: 192.168.1.1
         nameservers:
           addresses:
             - 8.8.8.8
             - 8.8.4.4
         access-points:
           "<SSID>":
             password: "<password>"
   ```
   - Replace `<SSID>` and `<password>` with your Wi-Fi credentials.
   - Update `addresses`, `gateway4`, and `nameservers` to match your network.

#### Explanation:
- **`dhcp4: no`**: Disables automatic IP assignment.
- **`addresses`**: Specifies the static IP and subnet mask (e.g., `/24` for 255.255.255.0).
- **`gateway4`**: Defines the network gateway (usually your router’s IP).
- **`nameservers`**: Specifies DNS servers for resolving domain names.

---

### Step 2: Apply the Configuration

To apply the changes:
```bash
sudo netplan apply
```

### Step 3: Verify the Static IP

Check the assigned IP:
```bash
ip addr
```

Ping a server to test connectivity:
```bash
ping -c 4 8.8.8.8
```

---

## Updating Wi-Fi or Static IP in the Future

### Changing Wi-Fi Networks
1. To connect to a different Wi-Fi network:
   ```bash
   nmcli dev wifi connect "<NewSSID>" password "<NewPassword>"
   ```

2. Update the Netplan configuration file if a static IP is required for the new network.

### Updating Static IP
1. Open the Netplan configuration file:
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```
2. Modify the `addresses`, `gateway4`, or `nameservers` sections as needed.
3. Apply the changes:
   ```bash
   sudo netplan apply
   ```
4. Verify the updates:
   ```bash
   ip addr
   ping -c 4 8.8.8.8
   ```

---

By following this guide, you can seamlessly switch Wi-Fi networks and ensure consistent connectivity with static IP configurations when necessary.
