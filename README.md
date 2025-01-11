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

### Step 2: Install Dependencies
Install the required dependencies for Kiwix:

```bash
sudo apt install build-essential cmake libboost-all-dev libssl-dev \
libcurl4-openssl-dev libxml2-dev zlib1g-dev qtbase5-dev \
libqt5webkit5-dev qtchooser qt5-qmake qtbase5-dev-tools \
libprotobuf-dev protobuf-compiler libjsoncpp-dev libsqlite3-dev \
libffi-dev libhidapi-dev libusb-1.0-0-dev libfuse-dev \
libminiupnpc-dev libboost-system-dev libboost-filesystem-dev \
libboost-program-options-dev libboost-thread-dev -y
```

### Step 3: Install Kiwix Tools
Install the Kiwix Tools package:

```bash
sudo apt install kiwix-tools -y
```

---

## Setting Up the Kiwix Library

### Step 4: Create a Library File

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

### Step 5: Download a Sample Book

Download a sample ZIM file to your library directory:

```bash
sudo wget -O /home/admin/kiwix-library/medlineplus.gov_en_all_2025-01.zim https://download.kiwix.org/zim/zimit/medlineplus.gov_en_all_2025-01.zim
```

### Step 6: Add the Book to Your Library

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

## Troubleshooting Tips
- **Permission Issues:** Ensure you are running commands with `sudo` if required.
- **Port Conflicts:** If port 80 is in use, choose another port with the `--port` flag.
- **File Download Problems:** Verify your internet connection and the URL of the ZIM file.

---

Enjoy using Kiwix on your Raspberry Pi for offline browsing!
