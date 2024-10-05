# Ticketing_system

A fully functional help desk system deployed on an Azure virtual machine, using osTicket 1.17.2. This documentation is guide through the installation, configuration, troubleshooting of the system, and provide a user guide for both employees and IT staff.

## Table of Contents

- [Requirements](#requirements)
- [Installation Process](#installation-process)
  - [Step 1: Create Azure VM](#step-1-create-azure-vm)
  - [Step 2: Install Apache](#step-2-install-apache)
  - [Step 3: Install MySQL](#step-3-install-mysql)
  - [Step 4: Install PHP](#step-4-install-php)
  - [Step 5: Download and Install osTicket](#step-5-download-and-install-osticket)
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

### Step 1: Create Azure VM
- Set up a virtual machine in Azure with Ubuntu 20.04 LTS.
- Enable SSH access for secure remote management.

### Step 2: Install Apache
```bash
sudo apt update && sudo apt install apache2 -y
```
Verify that Apache is running by visiting your VM's public IP.

### Step 3: Install MySQL
```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```
Create a MySQL database and user for osTicket.

### Step 4: Install PHP
```bash
sudo apt install php libapache2-mod-php php-mysql php-imap php-json php-curl php-mbstring php-xml php-gd php-intl -y
```

### Step 5: Download and Install osTicket
```bash
cd /var/www/html
sudo wget https://github.com/osTicket/osTicket/releases/download/v1.17.2/osTicket-v1.17.2.zip
sudo unzip osTicket-v1.17.2.zip
sudo mv upload/* .
sudo rm -rf upload osTicket-v1.17.2.zip
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

### Step 6: Complete the Installation
Visit `<your-VM-IP>/setup` in the browser and follow the web installer to configure osTicket.

Secure the installation by deleting the /setup directory and adjusting permissions:
```bash
sudo rm -rf setup/
sudo chmod 0644 include/ost-config.php
```

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

## Troubleshooting Guide

### Common Issues

- **Problem**: Users are not receiving email notifications.
  - **Solution**: Check the SMTP settings and ensure correct email credentials are configured.

- **Problem**: osTicket is not loading after installation.
  - **Solution**: Ensure Apache and MySQL services are running.

- **Problem**: Can’t log into the admin panel.
  - **Solution**: Reset the admin password using MySQL queries.

### Advanced Troubleshooting

- **Logs**: Check Apache and MySQL logs for server or database configuration issues.
- **Monitoring**: Set up Azure Monitor to keep track of your VM’s health and receive alerts for issues.

## User Guide

### For Employees/Users
- Access the client portal at `<osTicket-URL>`.
- Click on **Open a New Ticket**.
- Select a **Help Topic** (e.g., Password Reset) and fill out the required fields.
- Submit the ticket and you’ll receive an email confirmation with your ticket ID.

### For IT Staff
- Log in to the admin panel at `<osTicket-URL>/scp`.
- View assigned tickets under **Tickets → Open**.
- Respond to tickets by adding internal notes or replying directly to users.
- Update ticket statuses (Open, In Progress, Closed) as necessary.
- Resolve and close tickets when issues are fixed.

## Live Demo and Links
- **Live Demo**: [Click Here](http://52.156.20.186/)
- **Documentation Repo**: [GitHub Repository](https://github.com/chesahkalu/Ticketing_system)

## Built With
- **osTicket 1.17.2**
- **LAMP Stack (Linux, Apache, MySQL, PHP)**
- **Azure Cloud**

---