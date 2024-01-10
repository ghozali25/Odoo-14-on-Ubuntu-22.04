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
- create user postgresql (*odoo14 this your user)
  ```bash
  sudo su - postgres -c "createuser -s odoo14"
  ```
- create user for odoo
  ```bash
  sudo useradd -m -d /opt/odoo14 -U -r -s /bin/bash odoo14
  ```

