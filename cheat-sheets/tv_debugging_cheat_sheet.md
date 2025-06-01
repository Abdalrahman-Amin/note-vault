# 📺 TV Software Debugging Cheat Sheet

## 🎯 Quick Diagnosis: What Level Am I Debugging?

### 🔴 **LOW LEVEL** (Hardware/Kernel/System)
**Symptoms:**
- System crashes, reboots, freezes
- Hardware not detected
- Memory issues, kernel panics
- Driver problems
- Boot failures

**Primary Tools:** `dmesg`, `journalctl -k`, `/proc/`, `/sys/`

### 🟡 **MID LEVEL** (System Services/Framework)
**Symptoms:**
- Services failing to start
- Framework components not responding
- IPC communication issues
- Resource management problems

**Primary Tools:** `journalctl`, `systemctl`, WPE Framework logs

### 🟢 **HIGH LEVEL** (Applications/UI)
**Symptoms:**
- App crashes or hangs
- UI rendering issues
- JavaScript errors
- Network/streaming problems

**Primary Tools:** WPE Framework logs, app-specific logs, browser developer tools

---

## 🛠️ Essential Commands Toolkit

### **dmesg** - Kernel Ring Buffer
```bash
# Basic kernel messages
dmesg

# Real-time kernel messages
dmesg -w

# Filter by log level (emergency to debug: 0-7)
dmesg -l err,warn        # Errors and warnings only
dmesg -l 0,1,2,3         # Critical messages only

# Filter by facility
dmesg -f kern            # Kernel messages
dmesg -f daemon          # Daemon messages

# Timestamp format
dmesg -T                 # Human readable timestamps
dmesg -t                 # No timestamps

# Clear buffer
dmesg -c

# Useful for TV debugging
dmesg | grep -i "hdmi\|drm\|gpu\|memory\|usb\|ethernet"
```

### **journalctl** - Systemd Journal
```bash
# Basic viewing
journalctl                      # All logs
journalctl -f                   # Follow (tail -f equivalent)
journalctl -r                   # Reverse order (newest first)

# Time-based filtering
journalctl --since "2 hours ago"
journalctl --since "2023-01-01" --until "2023-01-02"
journalctl --since today
journalctl --since yesterday

# Priority filtering
journalctl -p err               # Error level and above
journalctl -p warning           # Warning level and above
journalctl -p 0..3              # Emergency to error levels

# Service-specific logs
journalctl -u wpeframework      # WPE Framework service
journalctl -u systemd-networkd  # Network service
journalctl -u your-app.service  # Your specific service

# Kernel logs only
journalctl -k                   # Same as dmesg but with journal features
journalctl -k -f                # Follow kernel logs

# Boot logs
journalctl -b                   # Current boot
journalctl -b -1                # Previous boot
journalctl --list-boots         # List all boots

# JSON output for parsing
journalctl -o json-pretty
journalctl -o json | jq '.'

# Disk usage
journalctl --disk-usage
```

### **WPE Framework** - TV Middleware Debugging
```bash
# WPE Framework service logs
journalctl -u wpeframework -f

# WPE Framework configuration
cat /etc/wpeframework/config.json

# Plugin status
curl -H "Content-Type: application/json" -X POST \
  -d '{"jsonrpc":"2.0","id":1,"method":"Controller.1.status"}' \
  http://127.0.0.1:9998/jsonrpc

# Activate/Deactivate plugins
curl -H "Content-Type: application/json" -X POST \
  -d '{"jsonrpc":"2.0","id":1,"method":"Controller.1.activate","params":{"callsign":"Netflix"}}' \
  http://127.0.0.1:9998/jsonrpc

# WebKit/Browser debugging
# Enable remote debugging in WPE config, then:
# Open browser to: http://TV_IP:9222
```

---

## 🔍 Debugging Workflow

### **Step 1: Initial Assessment**
```bash
# Quick system health check
uptime && free -h && df -h

# Check for obvious errors
dmesg | tail -20
journalctl -p err --since "10 minutes ago"

# Service status
systemctl status wpeframework
systemctl --failed
```

### **Step 2: Categorize the Issue**

#### **Hardware/Driver Issues** 🔴
```bash
# Check hardware detection
lspci -v                        # PCI devices
lsusb -v                        # USB devices
cat /proc/cpuinfo              # CPU info
cat /proc/meminfo              # Memory info

# Driver status
dmesg | grep -i "firmware\|driver\|error\|fail"
lsmod                          # Loaded modules
modinfo <module_name>          # Module details

# Hardware-specific logs
dmesg | grep -i "hdmi\|display\|audio\|network"
```

#### **System Service Issues** 🟡
```bash
# Service dependency check
systemctl list-dependencies wpeframework

# Resource usage
top -p $(pgrep wpeframework)
htop                           # Interactive process viewer

# IPC/Communication
netstat -tulpn                 # Network connections
ss -tulpn                      # Socket statistics
lsof -i :9998                  # WPE Framework port
```

#### **Application Issues** 🟢
```bash
# Application logs
journalctl -u wpeframework | grep -i "netflix\|youtube\|app_name"

# Memory leaks
cat /proc/$(pgrep wpeframework)/status | grep -i mem
pmap $(pgrep wpeframework)

# JavaScript/Web debugging
# Use Chrome DevTools via remote debugging
```

---

## 🚨 Common TV Software Issues & Solutions

### **Boot/Startup Issues**
```bash
# Check boot process
journalctl -b | grep -i "fail\|error\|timeout"
systemd-analyze                # Boot time analysis
systemd-analyze blame          # Slow services

# Fix: Check dependencies, configuration files
systemctl cat wpeframework     # View service file
```

### **HDMI/Display Issues**
```bash
# Display detection
xrandr                         # If X11 available
cat /sys/class/drm/*/status    # DRM status
dmesg | grep -i "hdmi\|drm\|display"

# Fix: Check EDID, resolution settings, HDCP
```

### **Memory Issues**
```bash
# Memory pressure
cat /proc/meminfo | grep -i "available\|free\|cached"
dmesg | grep -i "oom\|memory\|killed"

# Fix: Identify memory hogs, check for leaks
```

### **Network Issues**
```bash
# Network connectivity
ip a                           # Interface status
ping 8.8.8.8                   # Basic connectivity
curl -I http://www.google.com   # HTTP connectivity

# DNS resolution
nslookup google.com
cat /etc/resolv.conf

# Fix: Check network configuration, firewall
```

---

## 📋 Debugging Checklists

### **🔴 Low-Level Debug Checklist**
- [ ] Check `dmesg` for kernel errors
- [ ] Verify hardware detection (`lspci`, `lsusb`)
- [ ] Check driver loading (`lsmod`, `dmesg | grep driver`)
- [ ] Monitor resource usage (`free`, `df`, `/proc/loadavg`)
- [ ] Review boot logs (`journalctl -b`)

### **🟡 Mid-Level Debug Checklist**
- [ ] Check service status (`systemctl status`)
- [ ] Review service logs (`journalctl -u service`)
- [ ] Verify dependencies (`systemctl list-dependencies`)
- [ ] Check configuration files validity
- [ ] Monitor IPC/network connections

### **🟢 High-Level Debug Checklist**
- [ ] Check application logs
- [ ] Monitor memory usage per process
- [ ] Test network connectivity to services
- [ ] Use remote debugging tools
- [ ] Verify app configuration and permissions

---

## 🎯 Quick Commands for Emergency

### **System is Slow/Unresponsive**
```bash
top -c -d 1                    # What's using CPU
iotop                          # What's using I/O
dmesg | tail -50              # Recent kernel messages
journalctl -p err --since "5 minutes ago"
```

### **App Won't Start**
```bash
systemctl status wpeframework
journalctl -u wpeframework -n 50
curl -s http://127.0.0.1:9998/jsonrpc -d '{"jsonrpc":"2.0","method":"Controller.1.status","id":1}'
```

### **Network Issues**
```bash
ip route show                  # Routing table
netstat -i                     # Interface statistics
journalctl -u systemd-networkd -f
```

### **Memory Crisis**
```bash
cat /proc/meminfo | head -10
ps aux --sort=-%mem | head -10
dmesg | grep -i "killed\|oom"
```

---

## 💡 Pro Tips

1. **Always start with `dmesg` and `journalctl -p err`** - catches 80% of issues
2. **Use `journalctl -f`** while reproducing issues for real-time debugging
3. **Learn your TV's specific architecture** - ARM vs x86, GPU type, etc.
4. **Set up log rotation** - prevent disk space issues
5. **Create aliases** for commonly used debug commands
6. **Document patterns** - same issues often repeat

---

## 🔧 Advanced Debugging Tools

```bash
# System tracing
strace -p $(pgrep wpeframework)    # System calls
ltrace -p $(pgrep wpeframework)    # Library calls

# Performance profiling
perf top                           # CPU profiling
valgrind --tool=memcheck app       # Memory debugging

# Network debugging
tcpdump -i any port 9998          # Capture WPE traffic
wireshark                         # GUI network analysis
```

---

*💡 Remember: Start broad (system-wide), then narrow down (service-specific), then focus (app-specific)*