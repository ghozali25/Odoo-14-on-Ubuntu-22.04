# Odoo-14-on-Ubuntu-22.04
Install Odoo 14.0

- First Update Your apt
  ```bash
  sudo apt update
  ```
- Upgrade Your apt
  ```bash
  sudo apt upgrade -y
  ```
- Install Dependacies (fisrt check libssl1.1 *in terminal openssl version* if you have dont install)
  ```bash
  sudo apt install python3-pip python3-dev python3-venv python3-wheel libxml2-dev libpq-dev libjpeg8-dev liblcms2-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev build-essential git libssl-dev libffi-dev libmysqlclient-dev libjpeg-dev libblas-dev libatlas-base-dev libssl1.1
  ```
- install postgresql
  ```bash
  sudo apt install postgresql postgresql-contrib
  ```
- setting postgresql on your PC for automatically system boot
  ```bash
  sudo systemctl enable postgresql
  sudo systemctl start postgresql
  sudo systemctl status postgresql
  ```
- create user postgresql (*odoo14* this your user)
  ```bash
  sudo su - postgres -c "createuser -s odoo14"
  ```
- create user for odoo
  ```bash
  sudo useradd -m -d /opt/odoo14 -U -r -s /bin/bash odoo14
  ```
- download wkhtmltopdf and install
  ```bash
   wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
  ```
  install wkhtmltopdf
  ```bash
  chmod +x wkhtmltox_0.12.6.1-2.jammy_amd64.deb
  sudo apt install ./wkhtmltox_0.12.6.1-2.jammy_amd64.deb
  sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin/wkhtmltopdf
  ```
  and check after finish
  ```bash
  wkhtmltopdf --version
  ```
- Installing Odoo 14 on Ubuntu
  ```bash
  sudo su - odoo14
  ```
  download from odoo community (if you upgrade for new version you can replace name and branch 14 to new version)
  ```bash
  git clone https://www.github.com/odoo/odoo --depth 1 --branch 14.0 /opt/odoo14/odoo
  ```
  and exit after finish
  ```bash
  exit
  ```
- create new phyton virtual environtment
  ```bash
  sudo su
  cd /opt/odoo14
  python3 -m venv myodoo14-venv
  source myodoo14-venv/bin/activate
  ```
  and then install odoo14 dependencies (wait until finish)
  ```bash
  (venv) $ pip3 install wheel
  (venv) $ pip3 install -r odoo/requirements.txt
  ```
  and deactivate environtment
  ```bash
  deactivate
  ```
- make folder for custom addons
  ```bash
  mkdir /opt/odoo14/custom-addons
  ```
- let us exit to odoo14 user
  ```bash
  exit
  ```
- create configuration file odoo14
  ```bash
  sudo nano /etc/odoo14.conf
  ```
  - add this statement on odoo14.conf
    ```bash
    [options]
    ;This is the password that allows database operations:
    admin_passwd = admin
    db_host = False
    db_port = False
    db_user = odoo14
    db_password = False
    xmlrpc_port = 8069
    logfile = /var/log/odoo14/odoo.log
    addons_path = /opt/odoo14/odoo/addons,/opt/odoo14/custom-addons
    ```
- create configuration service system
  ```bash
  sudo nano /etc/systemd/system/odoo14.service
  ```
    - add this statement on odoo14.service
      ```bash
      [Unit]
      Description=Odoo14
      Requires=postgresql.service
      After=network.target postgresql.service

      [Service]
      Type=simple
      SyslogIdentifier=odoo14
      PermissionsStartOnly=true
      User=odoo14
      Group=odoo14
      ExecStart=/opt/odoo14/myodoo14-venv/bin/python3 /opt/odoo14/odoo/odoo-bin -c /etc/odoo14.conf
      StandardOutput=journal+console
      
      [Install]
      WantedBy=multi-user.target
      ```
  
  


  

