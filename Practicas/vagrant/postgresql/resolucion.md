### **1. Instalación de PostgreSQL en Xubuntu**

```sh
sudo apt update
sudo apt install wget openjdk-11-jdk -y
echo "deb https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
sudo apt update
sudo apt install dbeaver-ce -y
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

---

### **2. Crear Base de Datos y Usuario en PostgreSQL**

1. Accede al usuario de PostgreSQL:
    ```sh
    sudo -i -u postgres
    ```

2. Abre el intérprete de comandos de PostgreSQL:
    ```sh
    psql
    ```

3. Ejecuta los siguientes comandos para crear el usuario y la base de datos:

    ```sql
    CREATE USER david WITH PASSWORD 'david';
    ALTER USER david WITH SUPERUSER;
    CREATE DATABASE taller OWNER david;
    GRANT ALL PRIVILEGES ON DATABASE taller TO david;
    ```

4. Sal del intérprete de comandos:
    ```sh
    \q
    ```

5. Vuelve a tu usuario normal:
    ```sh
    exit
    ```

---

### **3. Configuración de Acceso Remoto**

#### Editar `postgresql.conf`

```sh
sudo nano /etc/postgresql/15/main/postgresql.conf
```

Modifica la línea:

```conf
listen_addresses = '*'
```

---

#### Editar `pg_hba.conf`

```sh
sudo nano /etc/postgresql/15/main/pg_hba.conf
```

Añade al final:

```conf
host    all             all             0.0.0.0/0               md5
```

---

#### Reiniciar PostgreSQL

```sh
sudo systemctl restart postgresql
```

---

### **4. Configuración del Firewall**

Permite el acceso al puerto 5432:

```sh
sudo ufw allow 5432/tcp
```