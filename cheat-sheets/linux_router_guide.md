# 🖥️ Linux Desktop as Router for TV + Bandwidth Limiting via `tc`

This guide shows how to use your **Linux desktop** as a **router** for your **smart TV**, assigning it an IP address via DHCP and **limiting its internet bandwidth** using `tc`.

## 📦 Requirements

* Linux desktop with 2 interfaces:
   * `eno2` (Ethernet) → Connected to TV
   * `wlo1` (Wi-Fi) → Connected to Internet
* Your TV must support Ethernet or USB-Ethernet
* Root (sudo) access

## ⚙️ Step 1: Assign a Static IP to LAN Interface

Replace `eno2` with your actual Ethernet interface (use `ip link show` to confirm).

```bash
sudo ip addr add 192.168.99.1/24 dev eno2
sudo ip link set eno2 up
```

## 🔁 Step 2: Enable IP Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

To make it permanent:

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## 🔀 Step 3: Enable NAT (Internet Sharing)

Replace `wlo1` with your internet interface:

```bash
sudo iptables -t nat -A POSTROUTING -o wlo1 -j MASQUERADE
sudo iptables -A FORWARD -i eno2 -o wlo1 -j ACCEPT
sudo iptables -A FORWARD -i wlo1 -o eno2 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

## 📡 Step 4: Setup DHCP Server

Install:

```bash
sudo apt install isc-dhcp-server
```

Edit config:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Append:

```conf
subnet 192.168.99.0 netmask 255.255.255.0 {
  range 192.168.99.10 192.168.99.20;
  option routers 192.168.99.1;
  option domain-name-servers 8.8.8.8;
}
```

Set interface:

```bash
sudo nano /etc/default/isc-dhcp-server
```

Set:

```bash
INTERFACESv4="eno2"
```

Restart:

```bash
sudo systemctl restart isc-dhcp-server
```

Reboot your TV or reconnect Ethernet.

Check logs:

```bash
sudo journalctl -u isc-dhcp-server
```

## ⏬ Step 5: Limit Bandwidth to TV using `tc`

Assume TV received `192.168.99.2`:

```bash
sudo tc qdisc del dev eno2 root || true

sudo tc qdisc add dev eno2 root handle 1: htb default 30

sudo tc class add dev eno2 parent 1: classid 1:1 htb rate 2mbit ceil 2mbit

sudo tc filter add dev eno2 protocol ip parent 1:0 prio 1 u32 \
  match ip dst 192.168.99.2 flowid 1:1
```

To remove limits:

```bash
sudo tc qdisc del dev eno2 root
```

## 📖 Interface Mapping Variables

| Purpose | Example Interface | How to Identify |
|---------|------------------|-----------------|
| Ethernet to TV | `eno2` | `ip link show` |
| Internet (Wi-Fi or other) | `wlo1` | Usually connected to internet |
| TV IP Address | `192.168.99.2` | Set by DHCP or manually assigned |

## 🧪 Testing

On the TV:

```bash
ping 192.168.99.1
ping 8.8.8.8
```

On the PC:

```bash
sudo tcpdump -i eno2
sudo journalctl -u isc-dhcp-server
```

## ✅ Done!

You now have a functioning Linux-based router with bandwidth control for your TV.

---

# 🔧 Enhanced Setup & Advanced Features

## Interface Detection Helper

Quick script to identify your interfaces:

```bash
# Quick script to identify your interfaces
echo "Available network interfaces:"
ip link show | grep -E "^[0-9]+:" | awk '{print $2}' | sed 's/://g'
echo -e "\nCurrent routes:"
ip route show
```

## Persistent iptables Rules

```bash
# Save current rules
sudo iptables-save > /tmp/iptables-rules

# Install persistence package
sudo apt install iptables-persistent

# Rules will be saved to /etc/iptables/rules.v4
sudo netfilter-persistent save
```

## 📊 Advanced Bandwidth Control

More sophisticated QoS setup with multiple traffic classes:

```bash
# More sophisticated QoS setup
sudo tc qdisc add dev eno2 root handle 1: htb default 30

# Main class - total bandwidth available
sudo tc class add dev eno2 parent 1: classid 1:1 htb rate 10mbit ceil 10mbit

# High priority (1mbit guaranteed, can burst to 5mbit)
sudo tc class add dev eno2 parent 1:1 classid 1:10 htb rate 1mbit ceil 5mbit prio 1

# Normal priority (2mbit guaranteed, can burst to 8mbit)  
sudo tc class add dev eno2 parent 1:1 classid 1:20 htb rate 2mbit ceil 8mbit prio 2

# Low priority (remaining bandwidth)
sudo tc class add dev eno2 parent 1:1 classid 1:30 htb rate 1mbit ceil 10mbit prio 3

# Filter for specific TV IP
sudo tc filter add dev eno2 protocol ip parent 1:0 prio 1 u32 \
  match ip dst 192.168.99.2 flowid 1:20
```

## 🛠️ Complete Automation Script

Save this as `router-setup.sh`:

```bash
#!/bin/bash
# router-setup.sh - Complete Linux Router Setup Script

# Configuration variables
LAN_INTERFACE="eno2"
WAN_INTERFACE="wlo1"
LAN_IP="192.168.99.1"
DHCP_RANGE_START="192.168.99.10"
DHCP_RANGE_END="192.168.99.20"

echo "🚀 Starting Linux Router Setup..."

# Enable IP forwarding
echo "📡 Enabling IP forwarding..."
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf

# Configure LAN interface
echo "🔧 Configuring LAN interface ${LAN_INTERFACE}..."
sudo ip addr add ${LAN_IP}/24 dev ${LAN_INTERFACE}
sudo ip link set ${LAN_INTERFACE} up

# Setup NAT
echo "🔀 Setting up NAT rules..."
sudo iptables -t nat -A POSTROUTING -o ${WAN_INTERFACE} -j MASQUERADE
sudo iptables -A FORWARD -i ${LAN_INTERFACE} -o ${WAN_INTERFACE} -j ACCEPT
sudo iptables -A FORWARD -i ${WAN_INTERFACE} -o ${LAN_INTERFACE} -m state --state RELATED,ESTABLISHED -j ACCEPT

# Save iptables rules
echo "💾 Saving iptables rules..."
sudo iptables-save > /tmp/router-iptables.rules

echo "✅ Router setup complete!"
echo "📋 Next steps:"
echo "   1. Setup DHCP server (Step 4 in guide)"
echo "   2. Configure bandwidth limiting (Step 5 in guide)"
echo "   3. Connect your TV via Ethernet"
```

Make it executable:

```bash
chmod +x router-setup.sh
sudo ./router-setup.sh
```

## 🔍 Monitoring and Troubleshooting

### Real-time Monitoring Commands

```bash
# Monitor DHCP assignments
sudo tail -f /var/log/syslog | grep dhcpd

# Watch network traffic
sudo iftop -i eno2

# Check current TC rules
sudo tc -s qdisc show dev eno2
sudo tc -s class show dev eno2

# Monitor connections
sudo netstat -rn

# Check interface statistics
ip -s link show eno2
```

### Traffic Analysis

```bash
# Install monitoring tools
sudo apt install iftop nethogs tcpdump

# Monitor bandwidth usage by process
sudo nethogs eno2

# Real-time interface monitoring
sudo iftop -i eno2

# Capture and analyze packets
sudo tcpdump -i eno2 -n
```

## 🚨 Security Enhancements

Basic firewall rules for added security:

```bash
# Allow SSH from local network only
sudo iptables -A INPUT -p tcp --dport 22 -s 192.168.99.0/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j DROP

# Rate limiting for DHCP (prevent DHCP exhaustion)
sudo iptables -A INPUT -p udp --dport 67 -m limit --limit 10/min -j ACCEPT
sudo iptables -A INPUT -p udp --dport 67 -j DROP

# Block common attack vectors
sudo iptables -A INPUT -p icmp --icmp-type ping -m limit --limit 1/second -j ACCEPT
sudo iptables -A INPUT -p icmp --icmp-type ping -j DROP
```

## 🧰 Advanced Optional Add-ons

### Traffic Shaping by Application

```bash
# Shape traffic based on port (example: limit video streaming)
sudo tc filter add dev eno2 protocol ip parent 1:0 prio 2 u32 \
  match ip dport 443 0xffff flowid 1:30

# Prioritize DNS traffic
sudo tc filter add dev eno2 protocol ip parent 1:0 prio 1 u32 \
  match ip dport 53 0xffff flowid 1:10
```

### Bandwidth Monitoring Script

```bash
#!/bin/bash
# bandwidth-monitor.sh

INTERFACE="eno2"
LOG_FILE="/var/log/router-bandwidth.log"

while true; do
    TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')
    RX_BYTES=$(cat /sys/class/net/$INTERFACE/statistics/rx_bytes)
    TX_BYTES=$(cat /sys/class/net/$INTERFACE/statistics/tx_bytes)
    
    echo "$TIMESTAMP - RX: $RX_BYTES bytes, TX: $TX_BYTES bytes" >> $LOG_FILE
    sleep 60
done
```

### Systemd Service for Auto-Start

Create `/etc/systemd/system/linux-router.service`:

```ini
[Unit]
Description=Linux Desktop Router
After=network.target

[Service]
Type=oneshot
ExecStart=/path/to/router-setup.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

Enable the service:

```bash
sudo systemctl enable linux-router.service
sudo systemctl start linux-router.service
```

## 🔧 Troubleshooting Common Issues

### DHCP Server Not Starting

```bash
# Check configuration syntax
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf

# Check interface binding
sudo systemctl status isc-dhcp-server

# Verify interface is up
ip addr show eno2
```

### No Internet Access on TV

```bash
# Check NAT rules
sudo iptables -t nat -L -n -v

# Verify IP forwarding
cat /proc/sys/net/ipv4/ip_forward

# Test connectivity from router
ping -I eno2 8.8.8.8
```

### Bandwidth Limiting Not Working

```bash
# Verify TC rules are applied
sudo tc -s qdisc show dev eno2
sudo tc -s class show dev eno2
sudo tc -s filter show dev eno2

# Check if traffic is hitting the right class
sudo tc -s class show dev eno2 | grep "class htb"
```

## 📚 Additional Resources

- **Traffic Control Documentation**: `man tc`
- **iptables Guide**: `man iptables`
- **DHCP Server Config**: `man dhcpd.conf`
- **Network Troubleshooting**: `man ip`, `man ss`, `man tcpdump`

---

**⚠️ Important Notes:**
- Always test in a safe environment first
- Keep backups of your original network configuration
- Monitor system resources when running as a router
- Consider using a dedicated router for production environments