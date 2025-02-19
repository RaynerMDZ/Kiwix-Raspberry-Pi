# Kiwix-Raspberry-Pi
Latest way to install Kiwix on Raspberry Pi. (It works with Pi Zero 2 W)

# How to Install Kiwix on Your Raspberry Pi

Kiwix allows you to access offline copies of websites like Wikipedia. This guide will walk you through installing and setting up Kiwix on your Raspberry Pi.

---

## Prerequisites
Before starting, make sure:

1. You have a Raspberry Pi with Raspberry Pi OS installed.
2. You have an active internet connection.
3. You are logged in as a user with `sudo` privileges.
4. My user is `admin`, if yours is different please change it in the below scrips.

---

## Installation Steps

### Step 1: Update and Upgrade Your System
First, ensure your system is up to date:

```bash
sudo apt update -y && sudo apt upgrade -y
```

### Step 2: Install Kiwix Tools
Install the Kiwix Tools package:

```bash
sudo apt install kiwix-tools -y
```

---

## Setting Up the Kiwix Library

### Step 3: Create a Library File

1. Create a directory to store your library:
   ```bash
   sudo mkdir /home/admin/kiwix-library
   ```

2. Create an empty `library.xml` file:
   ```bash
   sudo touch /home/admin/kiwix-library/library.xml
   ```

3. Open the file for editing:
   ```bash
   sudo nano /home/admin/kiwix-library/library.xml
   ```

4. Add the following content and save the file:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <library xmlns="http://www.kiwix.org/">
   </library>
   ```

### Step 4: Download a Sample Book

Download a sample ZIM file to your library directory:

```bash
sudo wget -O /home/admin/kiwix-library/archlinux_en_all_nopic_2022-12.zim https://download.kiwix.org/zim/other/archlinux_en_all_nopic_2022-12.zim
```

### Step 5: Add the Book to Your Library

Add the downloaded ZIM file to your `library.xml` file:

```bash
sudo kiwix-manage /home/admin/kiwix-library/library.xml add /home/admin/kiwix-library/medlineplus.gov_en_all_2025-01.zim -u https://download.kiwix.org/zim/zimit/medlineplus.gov_en_all_2025-01.zim
```

---

## Starting the Kiwix Server

Start the Kiwix server on port 80 using your library file:

```bash
sudo kiwix-serve --port=80 --library /home/admin/kiwix-library/library.xml
```

You can now access the Kiwix server by navigating to your Raspberry Pi's IP address in a web browser.

---

## Optional

To start Kiwix on port 80 after boot up:

```bash
sudo nano /etc/systemd/system/kiwix.service
```

Add the following:

```bash
[Unit]
Description=Kiwix Serve
After=network.target

[Service]
ExecStart=sudo /usr/bin/kiwix-serve --port=80 --library /home/admin/kiwix-library/library.xml
WorkingDirectory=/home/admin/kiwix-library
User=admin
Group=admin
Environment=DISPLAY=:0
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Finally, run the following:

```bash
sudo systemctl daemon-reload
sudo systemctl enable kiwix.service
sudo systemctl start kiwix.service
sudo systemctl status kiwix.service
```

---

## Troubleshooting Tips
- **Permission Issues:** Ensure you run commands with `sudo` if required.
- **Port Conflicts:** If port 80 is used, choose another port with the `--port` flag.
- **File Download Problems:** Verify your internet connection and the URL of the ZIM file.

---

Enjoy using Kiwix on your Raspberry Pi for offline browsing!
