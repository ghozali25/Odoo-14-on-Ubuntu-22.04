# Odoo-14.0-on-Ubuntu or Debian Based
<p align="center"><b>Created By Ahmad Ghozali</b></p>
  
Install Odoo 14.0
- Pertama update apt kamu
  ```bash
  sudo apt update
  ```
- Upgrade apt kamu
  ```bash
  sudo apt upgrade -y
  ```
- Install Dependecies (pertama cek libssl1.1 *di terminal ketik "openssl version"* jika sudah ada tidak perlu di instal)
  ```bash
  sudo apt install python3-pip python3-dev python3-venv python3-wheel libxml2-dev libpq-dev libjpeg8-dev liblcms2-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev build-essential git libssl-dev libffi-dev libmysqlclient-dev libjpeg-dev libblas-dev libatlas-base-dev libssl1.1
  ```
- install postgresql
  ```bash
  sudo apt install postgresql postgresql-contrib
  ```
- atur postgresql di PC kamu agar otomatis hidup saat booting
  ```bash
  sudo systemctl enable postgresql
  sudo systemctl start postgresql
  sudo systemctl status postgresql
  ```
- buat user baru postgresql (*odoo14* ini adalah user kamu)
  ```bash
  sudo su - postgres -c "createuser -s odoo14"
  ```
- buat user untuk odoo
  ```bash
  sudo useradd -m -d /opt/odoo14 -U -r -s /bin/bash odoo14
  ```
- download wkhtmltopdf dan install
  ```bash
   wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
  ```
  install wkhtmltopdf
  ```bash
  chmod +x wkhtmltox_0.12.6.1-2.jammy_amd64.deb
  sudo apt install ./wkhtmltox_0.12.6.1-2.jammy_amd64.deb
  sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin/wkhtmltopdf
  ```
  dan cek setelah selesai
  ```bash
  wkhtmltopdf --version
  ```
- masuk ke folder Odoo 14 kamu dengan syntax berikut
  ```bash
  sudo su - odoo14
  ```
  download dari odoo community (jika kamu mau upgrade ke versi terbaru tinggal ganti branch 14 ke versi terbaru)
  ```bash
  git clone https://www.github.com/odoo/odoo --depth 1 --branch 14.0 /opt/odoo14/odoo
  ```
  dan keluar dari odoo di terminal
  ```bash
  exit
  ```
- buat baru phyton virtual environtment
  ```bash
  sudo su
  cd /opt/odoo14
  python3 -m venv myodoo14-venv
  source myodoo14-venv/bin/activate
  ```
  dan install odoo14 dependencies (wait until finish)
  ```bash
  (venv) $ pip3 install wheel
  (venv) $ pip3 install -r odoo/requirements.txt
  ```
  dan nonaktifkan environment
  ```bash
  deactivate
  ```
- buat folder baru untuk custom addons
  ```bash
  mkdir /opt/odoo14/custom-addons
  ```
- sekarang keluar dari odoo14 user
  ```bash
  exit
  ```
- buat konfigurasi file odoo14
  ```bash
  sudo nano /etc/odoo14.conf
  ```
  - masukan perintah ini pada odoo14.conf
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
    and save (Ctrl+X => Yes (Y) => Enter
    
- buat konfigurasi di service system
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
      and save (Ctrl+X => Yes (Y) => Enter

- dan nyalakan ulang service odoo kamu
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable --now odoo14
  sudo systemctl status odoo14
  ```
- kamu bisa cek service odoo dengan ini
  ```bash
  sudo journalctl -u odoo14
  ```

- selamat kamu telah berhasil install Odoo.
  - kamu bisa cek di browser kamu dengan membuka localhost
    ```bash
    URL http://127.0.0.1:8069
    ```

<p align="center"><img src="https://github.com/ghozali25/Odoo-14-on-Ubuntu-22.04/blob/main/odoo14.png"></p>


<p align="center"><b>Thank For Attention</p>
  


  

