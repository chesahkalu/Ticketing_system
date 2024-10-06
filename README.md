# Ticketing_system

A fully functional help desk system deployed on an Azure virtual machine, using osTicket 1.17.2. This documentation is guide through the installation, configuration, troubleshooting of the system, and provide a user guide for both employees and IT staff.

## Table of Contents

- [Requirements](#requirements)
- [Installation Process](#installation-process)
    - [Step 1: Create Azure VM](#step-1-create-azure-vm)
    - [Step 2: Set Up the LAMP Stack](#step-2-set-up-the-lamp-stack)
        - [Install Apache](#install-apache)
        - [Install MySQL](#install-mysql)
        - [Install PHP](#install-php)
    - [Step 3: Download and Install osTicket](#step-3-download-and-install-osticket)
    - [Step 4: Configure MySQL Database for osTicket](#step-4-configure-mysql-database-for-osticket)
    - [Step 5: Configure osTicket](#step-5-configure-osticket)
    - [Step 6: Complete the Installation](#step-6-complete-the-installation)
- [System Configuration](#system-configuration)
  - [Departments](#departments)
  - [Agents](#agents)
  - [Help Topics (Ticket Categories)](#help-topics-ticket-categories)
- [Troubleshooting Guide](#troubleshooting-guide)
  - [Common Issues](#common-issues)
  - [Advanced Troubleshooting](#advanced-troubleshooting)
- [User Guide](#user-guide)
  - [For Employees/Users](#for-employeesusers)
  - [For IT Staff](#for-it-staff)
- [Live Demo and Links](#live-demo-and-links)
- [Built With](#built-with)

---

## Requirements

- Azure Virtual Machine (Ubuntu 20.04 LTS)
- LAMP Stack (Linux, Apache, MySQL, PHP)
- osTicket 1.17.2
- SMTP configuration (Gmail/SendGrid) for email notifications

---

## Installation Process


## Step 1: Create Azure VM

1. Create a new Virtual Machine in Azure.
2. Choose Ubuntu 20.04 LTS as the operating system.
3. Configure the network settings, allowing HTTP, HTTPS, and SSH.
4. Complete the VM creation process and log in via SSH.

## Step 2: Set Up the LAMP Stack

### Install Apache

1. **Update the System Packages**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. **Install Apache**
   ```bash
   sudo apt install apache2 -y
   ```
3. **Check if Apache is Running**
   - Visit your VM's public IP in a web browser:
     ```
     http://<your-VM-IP>
     ```
   - You should see the default Apache page.

### Install MySQL

1. **Install MySQL**
   ```bash
   sudo apt install mysql-server -y
   ```
2. **Secure MySQL Installation**
   ```bash
   sudo mysql_secure_installation
   ```
   - Follow the prompts and set a strong password for the MySQL root user.

### Install PHP

1. **Install PHP and Required Modules**
   ```bash
   sudo apt install php libapache2-mod-php php-mysql php-imap php-json php-curl php-mbstring php-xml php-gd php-intl -y
   ```
2. **Restart Apache to Load PHP**
   ```bash
   sudo systemctl restart apache2
   ```

## Step 3: Download and Install osTicket

1. **Navigate to the Apache Web Directory**
   ```bash
   cd /var/www/html
   ```
2. **Remove the Default Apache Index File**
   ```bash
   sudo rm index.html
   ```
3. **Install Unzip Utility**
   ```bash
   sudo apt install unzip -y
   ```
4. **Download osTicket**
   ```bash
   sudo wget https://github.com/osTicket/osTicket/releases/download/v1.17.2/osTicket-v1.17.2.zip
   ```
5. **Unzip the osTicket Package**
   ```bash
   sudo unzip osTicket-v1.17.2.zip
   sudo mv upload/* .
   sudo rm -rf upload osTicket-v1.17.2.zip
   ```
6. **Set Permissions**
   ```bash
   sudo chown -R www-data:www-data /var/www/html
   sudo chmod -R 755 /var/www/html
   ```

## Step 4: Configure MySQL Database for osTicket

1. **Log into MySQL**
   ```bash
   sudo mysql -u root -p
   ```
2. **Create a Database and User**
   ```sql
   CREATE DATABASE osticket;
   CREATE USER 'osticketuser'@'localhost' IDENTIFIED BY 'yourpassword';
   GRANT ALL PRIVILEGES ON osticket.* TO 'osticketuser'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```
   - Make sure to remember the database name, username, and password as you'll need them during the osTicket setup.

## Step 5: Configure osTicket

1. **Rename the Sample Configuration File**
   ```bash
   sudo cp include/ost-sampleconfig.php include/ost-config.php
   sudo chmod 0666 include/ost-config.php
   ```

## Step 6: Complete the Installation

1. **Finish Setup via the Web Installer**
   - Open your browser and navigate to:
     ```
     http://<your-VM-IP>/setup
     ```
   - Follow the setup wizard:
     - Enter your MySQL database information (DB name, user, and password).
     - Set up an admin account for the osTicket dashboard.
     - Complete the installation process.

2. **Secure the Installation**
   ```bash
   sudo rm -rf setup/
   sudo chmod 0644 include/ost-config.php
   ```

---

## Troubleshooting Tips

- **Apache Version Compatibility**: Ensure that the Apache version you installed is compatible with osTicket. The latest osTicket may require a specific version of Apache or specific modules.
- **MySQL Issues**: If you encounter issues connecting to the database, double-check that the username, password, and database name are correct, and make sure the MySQL server is running.
- **PHP Extensions**: Ensure all necessary PHP extensions are installed. Missing extensions may cause errors during the osTicket installation.
- **Permissions**: Incorrect permissions on the osTicket files or folders can lead to access issues. Make sure the Apache user (`www-data`) has ownership of the osTicket directory.

---

## System Configuration

### Departments
Navigate to **Admin Panel → Agents → Departments**.
Create departments like **Technical Support**, **Network Operations**, and assign department managers.

### Agents
Go to **Admin Panel → Agents → Add New Agent**.
Add support staff members and assign them to specific departments.

### Help Topics (Ticket Categories)
Go to **Admin Panel → Manage → Help Topics**.
Add common ticket categories like **Password Reset**, **Network Issues**, and set default priorities.

---

## Troubleshooting Guide

### Common Issues

- **Problem**: Users are not receiving email notifications.
  - **Solution**: Check the SMTP settings and ensure correct email credentials are configured.

- **Problem**: osTicket is not loading after installation.
  - **Solution**: Ensure Apache and MySQL services are running.

- **Problem**: Can’t log into the admin panel.
  - **Solution**: Reset the admin password using MySQL queries.

---

### Advanced Troubleshooting

- **Logs**: Check Apache and MySQL logs for server or database configuration issues.
- **Monitoring**: Set up Azure Monitor to keep track of your VM’s health and receive alerts for issues.

---

## User Guide

### For Employees/Users
- Access the client portal at `<osTicket-URL>`.
- Click on **Open a New Ticket**.
- Select a **Help Topic** (e.g., Password Reset) and fill out the required fields.
- Submit the ticket and you’ll receive an email confirmation with your ticket ID.

---

### For IT Staff
- Log in to the admin panel at `<osTicket-URL>/scp`.
- View assigned tickets under **Tickets → Open**.
- Respond to tickets by adding internal notes or replying directly to users.
- Update ticket statuses (Open, In Progress, Closed) as necessary.
- Resolve and close tickets when issues are fixed.

---

## Live Demo and Links
- **Live Demo**: [Click Here](http://52.156.20.186/)
- **Documentation Repo**: [GitHub Repository](https://github.com/chesahkalu/Ticketing_system)

## Built With
- **osTicket 1.17.2**
- **LAMP Stack (Linux, Apache, MySQL, PHP)**
- **Azure Cloud**

---