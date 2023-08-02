# CSI Linux modification
This document contains some small extensions to the comprehensive CSI Linux distribution. These mainly consist of installing additional software to meet personal needs during investigation cases.

Everything has been tested with CSI Linux 2023 (as of July 2023).

## Prerequisites
- Install XRDP (to connect via RDP)
  ```bash
  sudo apt install xrdp
  ```
- Set the correct timezone
  ```bash
  sudo timedatectl set-timezone Europe/Berlin
  ```
- Install vim
  ```bash
  sudo apt install vim
  ```
- Install docker and docker-compose
  ```bash
  sudo apt install docker.io docker-compose
  sudo usermod -aG docker csi
  #logout or reboot
  ```
 - Create additional directories
   ```bash
   mkdir /home/csi/Desktop/Additional\ Tools
   mkdir /home/csi/.addres/
   mkdir -p /home/csi/tools
   ```

## MobSF
- Get MobSF
  ```bash
  docker pull opensecurity/mobile-security-framework-mobsf
  ```
- Create a shortcut within the "Additional Tools" folder on the Desktop
  ```bash
  mkdir -p /home/csi/.addres/mobsf
  wget -O /home/csi/.addres/mobsf/logo.png https://avatars.githubusercontent.com/u/25052637?s=200\&v=4

  cat << EOF > /home/csi/Desktop/Additional\ Tools/Start\ MobSF.desktop
  [Desktop Entry]
  Version=1.0
  Type=Application
  Name=Start MobSF
  Comment=Run the docker instance with MobSF
  Exec=docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
  Icon=/home/csi/.addres/mobsf/logo.png
  Path=
  Terminal=true
  StartupNotify=true
  EOF

  chmod +x /home/csi/Desktop/Additional\ Tools/Start\ MobSF.desktop
  ```

## Zui
- Get the latest package from https://github.com/brimdata/zui/releases/ and install, e.g.
  ```bash
  wget -O /tmp/zui.dpkg https://github.com/brimdata/zui/releases/download/v1.1.0/zui_1.1.0_amd64.deb
  sudo dpkg -i /tmp/zui.dpkg
  ```

- Create a shortcut within the "Additional Tools" folder on the Desktop
  ```bash
  cp /usr/share/applications/zui.desktop /home/csi/Desktop/Additional\ Tools/
  chmod +x /home/csi/Desktop/Additional\ Tools/zui.desktop
  rm -f /tmp/zui.dpkg
  ```

## oletools
```bash
sudo -H pip install -U oletools[full]
```

## unoconv
```bash
sudo apt install unoconv
```

## ViperMonkey
```bash
mkdir -p /home/csi/tools/
cd /home/csi/tools
git clone https://github.com/decalage2/ViperMonkey
ln -s /home/csi/tools/ViperMonkey /home/csi/Desktop/Additional\ Tools/ViperMonkey
```

## Aurora
- Get the latest package from https://github.com/cyb3rfox/Aurora-Incident-Response/releases and install, e.g.
  ```bash
  wget -O /tmp/aurora.zip https://github.com/cyb3rfox/Aurora-Incident-Response/releases/download/0.6.6/Aurora-linux-x64-0.6.6.zip
  unzip /tmp/aurora.zip -d /home/csi/tools/
  rm -f /tmp/aurora.zip
  mkdir -p /home/csi/.addres/aurora
  wget -O /home/csi/.addres/aurora/logo.png https://raw.githubusercontent.com/cyb3rfox/Aurora-Incident-Response/master/src/icon/aurora.png
  ```

- Create a shortcut within the "Additional Tools" folder on the Desktop
  ```bash
  cat << EOF > /home/csi/Desktop/Additional\ Tools/Aurora.desktop
  [Desktop Entry]
  Version=1.0
  Type=Application
  Name=Aurora
  Comment=Aurora Incident Response Documentation
  Exec=/home/csi/tools/Aurora-linux-x64/Aurora
  Icon=/home/csi/.addres/aurora/logo.png
  Path=
  Terminal=false
  StartupNotify=true
  EOF

  chmod +x /home/csi/Desktop/Additional\ Tools/Aurora.desktop
  ```

## Zeek
- https://docs.zeek.org/en/master/install.html
- Check if CSI Linux is compatible (is true for CSI Linux 2023)
- Add the repository for Ubuntu 22.04 LTS and install
  ```bash
  echo 'deb http://download.opensuse.org/repositories/security:/zeek/xUbuntu_22.04/ /' | sudo tee /etc/apt/sources.list.d/security:zeek.list
  cd /tmp/
  curl -fsSL https://download.opensuse.org/repositories/security:zeek/xUbuntu_22.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/security_zeek.gpg > /dev/null
  sudo apt update
  sudo apt install zeek-lts

  echo 'export PATH=$PATH:/opt/zeek/bin/' >> /home/csi/.bashrc
  ```

## Rita
- Preparing to use RITA via Docker
  ```bash
  mkdir -p /home/csi/tools/ritaconf
  docker pull quay.io/activecm/rita
  wget -O /home/csi/tools/ritaconf/rita.yaml https://raw.githubusercontent.com/activecm/rita/master/etc/rita.yaml
  wget -O /home/csi/tools/ritaconf/docker-compose.yaml https://raw.githubusercontent.com/activecm/rita/master/docker-compose.yml
  sed -i 's#ConnectionString: mongodb://localhost:27017#ConnectionString: mongodb://db:27017#' /home/csi/tools/ritaconf/rita.yaml
  ```
- To run RITA set the following variables
  ```bash
  export CONFIG=/home/csi/tools/ritaconf/rita.yaml
  export LOGS=</path/to/zeek/logs/>
  ```
- Check if it works
  ```bash
  docker-compose -f /home/csi/tools/ritaconf/docker-compose.yaml run --rm rita help
  ```

## RepCheck
```bash
cd /home/csi/tools/
git clone https://github.com/riotsecurity/RepCheck
chmod +x /home/csi/tools/RepCheck/repcheck.py
ln -s /home/csi/tools/RepCheck /home/csi/Desktop/Additional\ Tools/RepCheck
```
