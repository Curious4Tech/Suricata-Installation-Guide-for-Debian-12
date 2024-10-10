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

2. Install dependencies:
   ```
   sudo apt install -y libpcre3 libpcre3-dbg libpcre3-dev build-essential autoconf automake libtool libpcap-dev libnet1-dev libyaml-0-2 libyaml-dev pkg-config zlib1g zlib1g-dev libcap-ng-dev libmagic-dev libjansson-dev libgeoip-dev python3-yaml rustc cargo

3. Install Suricata:
   ```
   sudo apt install -y suricata
   ```

7. Verify the installation:
   ```
   suricata --version
   ```
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
There are many possible configuration options, we focus on the setup of the `HOME_NET` variable and the network interface configuration. The `HOME_NET` variable should include, in most scenarios, the IP address of the monitored interface and all the local networks in use. The default already includes the RFC 1918 networks. 

<img width="850" alt="image" src="https://github.com/user-attachments/assets/f601dbdc-92bd-456a-b359-9ea666b0a55d">


In this example `192.168.10.9` is already included within `192.168.10.0/24`. If no other networks are used the other predefined values can be removed.

In this example the interface name is `enp0s3` so the interface name in the af-packet section needs to match. An example interface config might look like this:

Capture settings:
```
af-packet:
    - interface: enp0s3
      cluster-id: 99
      cluster-type: cluster_flow
      defrag: yes
      use-mmap: yes
      tpacket-v3: yes
```
Check that Suricata is running properly:
```
sudo systemctl status suricata
```
<img width="960" alt="image" src="https://github.com/user-attachments/assets/28223828-821f-4c7a-8304-7f8d1d9d84cd">

## Updating Suricata Rules
Suricata uses Signatures to trigger alerts so it's necessary to install those and keep them updated. Signatures are also called rules, thus the name rule-files. With the tool suricata-update rules can be fetched, updated and managed to be provided for Suricata.
In this guide we just run the default mode which fetches the ET Open ruleset:
   ```
   sudo suricata-update
   ```
<img width="942" alt="image" src="https://github.com/user-attachments/assets/7c76fb8c-b548-4ea8-b07c-44e1fdfd62be">


Afterwards the rules are installed at `/var/lib/suricata/rules` which is also the default at the config and uses the sole `suricata.rules` file.


Restart the Suricata service:

With the rules installed, Suricata can run properly and thus we restart it:
   ```
   sudo systemctl restart suricata
   ```

## Troubleshooting

If you encounter any issues, check the Suricata log file:
```
sudo tail -f /var/log/suricata/suricata.log
```

For more detailed information, refer to the [official Suricata documentation](https://suricata.readthedocs.io/).
