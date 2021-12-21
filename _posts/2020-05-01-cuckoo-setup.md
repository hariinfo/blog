---
layout: post
title: Cuckoo Automated Malware Sandbox - Part 1
date: 2020-04-30
---

## 1. Install packages on the host (Ubuntu)
```bash
sudo apt-get update
sudo apt-get install python python-pip python-dev libffi-dev libssl-dev
sudo apt-get install python-virtualenv python-setuptools
sudo apt-get install libjpeg-dev zlib1g-dev swig
```

### 1.2 Optional Components
- We have to install the following components for cuckoo Web UI

    ```bash
    sudo apt-get install mongodb
    sudo apt-get install postgresql libpq-dev
    ```

### 1.2 Install Virtualbox
- Install Oracle VM on the guest machine and create an instance of Windows 7 or 10
- Create a new user for cuckoo and add it to the VM user group
    ```bash
    sudo adduser cuckoo
    sudo usermod -a -G vboxusers cuckoo
    ```
- Setup VirtualBox nework
    ```bash
    VBoxManage hostonlyif create
    VBoxManage hostonlyif ipconfig malware_net --ip 192.168.56.1 --netmask 255.255.255.0
    ```
- Configure Windows virtual box to use "Host-only Adapter" and "malware_net" as the Network name

## 2. Install packages on the guest (Windows)
- Install python and copy the 
- This is an optional package and enables cuckoo to collect screenshots from the guest VM
    ```bash
    python -m pip install --upgrade pip
    python -m pip install --upgrade Pillow
    ```

## 3. Run cuckoo
- Open a terminal and activate virtual env before launching cuckoo
    ```bash
    virtualenv venv
    . venv/bin/activate
    cuckoo -d
    ```
- From another terminal launch the Cuckoo UI as follows
    ```bash
    cuckoo web runserver 127.0.0.1:8080
    ```
## 4. Credits
<https://cuckoosandbox.org/>
<https://yojimbosecurity.ninja/setting-up-cuckoo/>
<https://gist.github.com/braimee/bf570a62f53f71bad1906c6e072ce993>
