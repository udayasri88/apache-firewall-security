# Apache Web Server Security & Monitoring Using Linux Firewall

**Author:** Udayasri Parvatha
**Environment:** Kali Linux (VirtualBox lab)

## Overview

This project demonstrates the deployment and basic security hardening of an Apache web server on Kali Linux. It covers configuring Apache to host a custom webpage, allowing access from other devices on the network, implementing firewall rules using UFW, restricting unwanted traffic, blocking a specific IP address, and monitoring Apache access/error logs.

The goal was to understand how a Linux server can be configured, protected, and monitored in a small network environment — core skills for a SOC / cybersecurity role.

## Objectives

- Install and configure the Apache web server
- Host a custom webpage using Apache
- Access the webpage from another system (Windows, Android)
- Configure UFW as a host-based firewall
- Allow HTTP traffic on TCP port 80
- Block unwanted incoming traffic
- Block a specific IP address
- Monitor Apache access and error logs
- Understand basic server security and access control

## Lab Environment

| Component  | OS           | IP Address       |
|------------|--------------|-------------------|
| Server     | Kali Linux   | 192.168.43.7      |
| Client 1   | Windows      | 192.168.43.218    |
| Client 2   | Android      | 192.168.43.1      |

![Network config and curl test](screenshots/01-network-config-curl-test.jpg)

## 1. Apache Web Server Verification

The Apache server was tested locally using:
```bash
curl http://localhost
```
A response from Apache confirmed the web server was functioning correctly.

- Webpage directory: `/var/www/html/`
- Default webpage: `/var/www/html/index.html`

![Windows ipconfig showing client IP](screenshots/02-windows-ipconfig.png)

## 2. Accessing the Webpage from Other Devices

**From Windows**, the webpage was accessed at `http://192.168.43.7`, confirming communication between the Windows client and the Kali Apache server.

![Apache custom webpage accessed from Windows](screenshots/03-apache-webpage-windows.png)

**From Android**, the same URL (`http://192.168.43.7`) was used and access was successful.

![Apache custom webpage accessed from Android](screenshots/04-apache-webpage-android.png)

## 3. UFW Firewall Installation & Configuration

UFW (Uncomplicated Firewall) was installed to provide host-based firewall protection:
```bash
apt install ufw -y
ufw status
```
Initially the firewall was inactive. It was then set to deny incoming by default and enabled:
```bash
ufw default deny incoming
ufw enable
ufw status verbose
```

![UFW installation and status output](screenshots/05-ufw-firewall-setup.jpg)

## 4. Blocking a Specific IP Address

A specific source IP was blocked using:
```bash
ufw insert 1 deny from 192.168.43.20
```
This blocked all incoming traffic from `192.168.43.20`. The rule was verified with:
```bash
ufw status numbered
```
After the rule was applied, the blocked host could no longer reach the server (connection timed out).

![Connection timeout after blocking the IP](screenshots/06-ip-blocked-verification.png)

## 5. Apache Access Log Monitoring

Apache records all client requests in:
```
/var/log/apache2/access.log
```
The log was monitored in real time using:
```bash
tail -f /var/log/apache2/access.log
```
This showed live client activity — request methods, source IPs, user agents, and HTTP response codes — as devices accessed the webpage.

![Apache access log monitored in real time](screenshots/07-apache-access-logs.jpg)

## Conclusion

This project provided practical experience configuring and securing a Linux-based web server. Apache was successfully deployed to host a custom webpage, while UFW was used to control incoming network traffic — explicitly allowing HTTP access for the lab network while restricting unwanted traffic. Apache logs were monitored to observe client activity and HTTP responses.

The project demonstrates the fundamental relationship between Linux administration, networking, firewall security, web servers, and security monitoring — all core skills for a cybersecurity/SOC role.

## Tools Used

- Kali Linux, Apache2, UFW (Uncomplicated Firewall)
- VirtualBox (lab environment)
- curl, tail (CLI monitoring)
