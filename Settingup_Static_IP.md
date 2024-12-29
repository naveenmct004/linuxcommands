# Setting Up a Static IP Address on Linux

A static IP address is a fixed IP assigned to your system, ensuring that it doesn’t change after a reboot or over time. This is useful for servers, printers, or any device that needs consistent network access.

---

## Why Do We Need a Static IP Address?

1. **Consistent Addressing:**
   - Ensures that the system always has the same IP address, simplifying connections and configurations.

2. **Simplified Networking:**
   - Avoids the need to update configurations (e.g., firewall rules, SSH, DNS) if the IP changes.

3. **Essential for Services:**
   - Required for hosting services like web servers, file servers, or database servers.

4. **Avoid Conflicts:**
   - Prevents issues caused by dynamic IP changes, especially in networks with multiple devices.

---

## How to Set Up a Static IP

### **Step 1: Identify Your Network Interface**

List all network interfaces to determine which one needs a static IP:
```bash
ip addr
```

Look for interfaces like `eth0`, `ens33`, or `wlan0` with an active connection.

---

### **Step 2: Backup Current Configuration**

Before making changes, back up the current network configuration:
```bash
sudo cp /etc/netplan/*.yaml /etc/netplan/backup.yaml
```

---

### **Step 3: Configure the Static IP**

1. Open the Netplan configuration file (common on Ubuntu):
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```

2. Update the file to include the static IP configuration:

   Example:
   ```yaml
   network:
     version: 2
     renderer: networkd
     ethernets:
       eth0:
         dhcp4: no
         addresses:
           - 192.168.1.100/24
         gateway4: 192.168.1.1
         nameservers:
           addresses:
             - 8.8.8.8
             - 8.8.4.4
   ```

   - Replace `eth0` with your network interface name.
   - Replace `192.168.1.100` with the desired static IP.
   - Replace `192.168.1.1` with your network's gateway.
   - Replace `8.8.8.8` and `8.8.4.4` with preferred DNS servers.

3. Apply the configuration:
   ```bash
   sudo netplan apply
   ```

---

### **Step 4: Verify the Configuration**

Check the IP address to ensure the changes were applied:
```bash
ip addr
```

Ping a server to verify connectivity:
```bash
ping -c 4 8.8.8.8
```

---

## How to Update the Static IP in the Future

1. **Edit the Netplan Configuration File:**
   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```

2. **Modify the Required Values:**
   - Change `addresses`, `gateway4`, or `nameservers` as needed.

3. **Apply the Changes:**
   ```bash
   sudo netplan apply
   ```

4. **Verify the New Configuration:**
   ```bash
   ip addr
   ping -c 4 8.8.8.8
   ```

---

By setting up a static IP address, you ensure consistent network connectivity and simplify network management, particularly for servers and critical systems.
