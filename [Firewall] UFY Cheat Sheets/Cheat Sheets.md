Here’s a **UFW (Uncomplicated Firewall)** cheat sheet for managing firewall rules on Ubuntu. UFW simplifies the process of managing **iptables**.

### **Basic Commands**

- **Check UFW status:**
  ```bash
  sudo ufw status
  ```
  To get more detailed status information:
  ```bash
  sudo ufw status verbose
  ```

- **Enable UFW:**
  ```bash
  sudo ufw enable
  ```
  Enables the firewall and starts it on boot.

- **Disable UFW:**
  ```bash
  sudo ufw disable
  ```
  Disables the firewall.

- **Reset UFW (Remove all rules):**
  ```bash
  sudo ufw reset
  ```
  Resets UFW to its default state, removing all rules.

### **Allowing and Denying Traffic**

- **Allow a specific port (e.g., 80 for HTTP):**
  ```bash
  sudo ufw allow 80/tcp
  ```

- **Allow a range of ports:**
  ```bash
  sudo ufw allow 1000:2000/tcp
  ```

- **Allow a specific port from a specific IP:**
  ```bash
  sudo ufw allow from 192.168.1.100 to any port 80
  ```

- **Deny a specific port (e.g., 22 for SSH):**
  ```bash
  sudo ufw deny 22
  ```

- **Allow a specific service by name (e.g., SSH, HTTP, etc.):**
  ```bash
  sudo ufw allow ssh
  sudo ufw allow http
  ```

- **Allow all incoming traffic (Not recommended for production):**
  ```bash
  sudo ufw allow in from any
  ```

- **Deny all incoming traffic:**
  ```bash
  sudo ufw default deny incoming
  ```

- **Allow all outgoing traffic:**
  ```bash
  sudo ufw default allow outgoing
  ```

### **Advanced Rules**

- **Allow a specific port for UDP traffic:**
  ```bash
  sudo ufw allow 123/udp
  ```

- **Allow a specific port for TCP traffic:**
  ```bash
  sudo ufw allow 443/tcp
  ```

- **Allow a port from a specific IP (e.g., IP address `192.168.0.1` for port 3000):**
  ```bash
  sudo ufw allow from 192.168.0.1 to any port 3000
  ```

- **Allow a port from a specific network range (e.g., from `192.168.1.0/24`):**
  ```bash
  sudo ufw allow from 192.168.1.0/24 to any port 8080
  ```

- **Allow incoming traffic from a subnet (e.g., `192.168.0.0/24`):**
  ```bash
  sudo ufw allow from 192.168.0.0/24
  ```

- **Deny traffic from a specific IP:**
  ```bash
  sudo ufw deny from 192.168.1.100
  ```

### **Managing Application Profiles**

- **List all available application profiles (services):**
  ```bash
  sudo ufw app list
  ```

- **Allow traffic for a specific application profile (e.g., "OpenSSH"):**
  ```bash
  sudo ufw allow OpenSSH
  ```

- **Deny traffic for a specific application profile (e.g., "Apache"):**
  ```bash
  sudo ufw deny Apache
  ```

### **Checking and Logging Rules**

- **View all UFW rules:**
  ```bash
  sudo ufw show added
  ```

- **Check specific UFW rule:**
  ```bash
  sudo ufw status numbered
  ```

- **Enable UFW logging (if not already enabled):**
  ```bash
  sudo ufw logging on
  ```

- **Disable UFW logging:**
  ```bash
  sudo ufw logging off
  ```

### **Other Useful Commands**

- **Delete a specific rule by its number (e.g., rule number 2):**
  ```bash
  sudo ufw delete 2
  ```

- **Disable UFW on startup:**
  ```bash
  sudo ufw disable
  ```

- **Allow and then enable UFW if it's not enabled already:**
  ```bash
  sudo ufw allow 80/tcp
  sudo ufw enable
  ```

### **Checking UFW Status with Details**

- **Get detailed status of UFW:**
  ```bash
  sudo ufw status verbose
  ```

### **Example Use Case**

#### Allow HTTP (port 80) and SSH (port 22):

1. Allow HTTP and SSH:
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 22/tcp
   ```

2. Enable UFW:
   ```bash
   sudo ufw enable
   ```

3. Verify UFW status:
   ```bash
   sudo ufw status
   ```

This should allow both HTTP and SSH traffic while blocking other ports.

---

### **Summary of Key Commands:**

- **Enable UFW**: `sudo ufw enable`
- **Allow a port**: `sudo ufw allow <port>/tcp`
- **Deny a port**: `sudo ufw deny <port>/tcp`
- **Check status**: `sudo ufw status`
- **Reset UFW**: `sudo ufw reset`
- **View all rules**: `sudo ufw show added`
