# Suricata-Installation-Guide-for-Debian-12
A comprehensive guide for installing and configuring Suricata on Debian 12. Includes step-by-step instructions, troubleshooting tips, and best practices for setting up this powerful network security monitoring tool.

# Suricata Installation Guide for Debian 12

This guide provides step-by-step instructions for installing Suricata on Debian 12.

## Prerequisites

- A Debian 12 system with root or sudo access
- Internet connection

## Installation Steps

1. Update the system:
   ```
   sudo apt update && sudo apt upgrade -y
   ```
   <img width="958" alt="image" src="https://github.com/user-attachments/assets/542bc334-0e6f-46de-8d43-4679bfcaa4d0">



2. Install dependencies:
   ```
   sudo apt install -y libpcre3 libpcre3-dbg libpcre3-dev build-essential autoconf automake libtool libpcap-dev libnet1-dev libyaml-0-2 libyaml-dev pkg-config zlib1g zlib1g-dev libcap-ng-dev libmagic-dev libjansson-dev libgeoip-dev python3-yaml rustc cargo

<img width="960" alt="image" src="https://github.com/user-attachments/assets/3517c10c-d6d3-4e45-9617-fa3c9e14ff1d">  




3. Install Suricata:
   ```
   sudo apt install -y suricata
   ```
  <img width="955" alt="image" src="https://github.com/user-attachments/assets/a72eafd9-08da-478d-8d86-f2c550ce2ea9">

7. Verify the installation:
   ```
   suricata --version
   ```
 <img width="888" alt="image" src="https://github.com/user-attachments/assets/5fb71948-8a0c-4588-91b6-50dc4424cf1e">

8. Enable and start the Suricata service:
   ```
   sudo systemctl enable suricata
   sudo systemctl start suricata
   ```

## Configuration

After installing Suricata, you need to configure it. The main configuration file is located at /etc/suricata/suricata.yaml. You can edit this file to set up network interfaces, rules, and logging options:
 ```
sudo nano /etc/suricata/suricata.yaml
```
There are many possible configuration options, we focus on the setup of the HOME_NET variable and the network interface configuration. The HOME_NET variable should include, in most scenarios, the IP address of the monitored interface and all the local networks in use. The default already includes the RFC 1918 networks. In this example 10.0.0.23 is already included within 10.0.0.0/8. If no other networks are used the other predefined values can be removed.

In this example the interface name is enp1s0 so the interface name in the af-packet section needs to match. An example interface config might look like this:

Capture settings:
```
af-packet:
    - interface: enp1s0
      cluster-id: 99
      cluster-type: cluster_flow
      defrag: yes
      use-mmap: yes
      tpacket-v3: yes
```

## Updating Suricata Rules

To update Suricata rules:

1. Stop the Suricata service:
   ```
   sudo systemctl stop suricata
   ```

2. Update the rules:
   ```
   sudo suricata-update
   ```

3. Restart the Suricata service:
   ```
   sudo systemctl start suricata
   ```

## Troubleshooting

If you encounter any issues, check the Suricata log file:
```
sudo tail -f /var/log/suricata/suricata.log
```

For more detailed information, refer to the [official Suricata documentation](https://suricata.readthedocs.io/).
