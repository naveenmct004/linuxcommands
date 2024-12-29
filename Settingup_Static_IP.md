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

- Look for interfaces like `eth0`, `ens33`, or `wlan0` with an active connection. These names represent network adapters on your system.
- Identify the adapter currently in use, usually marked with an `UP` state and an assigned IP address.

---

### **Step 2: Backup Current Configuration**

Before making changes, back up the current network configuration:
```bash
sudo cp /etc/netplan/*.yaml /etc/netplan/backup.yaml
```

#### Why Do We Need to Take a Backup?
- A backup ensures you can restore the original configuration in case of mistakes or network issues.
- This is especially critical on headless systems (like servers) where losing network access can require physical access to fix.

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

   #### Explanation:
   - **`eth0`**: Replace this with your network interface name (found in Step 1).
   - **`dhcp4: no`**: Disables DHCP for this interface, enabling manual IP assignment.
   - **`addresses`**: Specifies the static IP and subnet mask (e.g., `/24` for a 255.255.255.0 mask).
   - **`gateway4`**: This is the IP address of your router (network gateway), which routes traffic between your local network and the internet.
   - **`nameservers`**: DNS servers resolve domain names (like `google.com`) into IP addresses. Common DNS servers include Google’s (8.8.8.8, 8.8.4.4) or Cloudflare’s (1.1.1.1).

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

#### Why Verify?
- Ensures the static IP is correctly set up and can communicate with the network and the internet.

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

### Updating IP After Changing the Router

When changing your router, you may need to:

1. **Determine the New Network Configuration:**
   - Check the new router’s IP address (e.g., 192.168.0.1).
   - Identify the new subnet (e.g., 192.168.0.0/24).

2. **Update the Gateway and IP Address:**
   - Edit `/etc/netplan/01-netcfg.yaml` and update `gateway4` to the new router IP.
   - Adjust `addresses` to match the new subnet (e.g., `192.168.0.100/24`).

3. **Apply the Changes:**
   ```bash
   sudo netplan apply
   ```

4. **Verify Connectivity:**
   ```bash
   ip addr
   ping -c 4 8.8.8.8
   ```

By setting up a static IP address and understanding how to manage it, you ensure consistent network connectivity and simplify network management, particularly for servers and critical systems.
