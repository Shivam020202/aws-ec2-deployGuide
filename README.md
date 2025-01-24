# AWS EC2 Website Deployment Guide

## Prerequisites
1. PuTTY - SSH client for Windows
   - Download from: https://putty.org/
2. WinSCP - File transfer client
   - Download from: https://winscp.net/
3. Required credentials:
   - .pem file (private key)
   - Server address
   - Domain name

## Initial Setup

### Converting .pem to .ppk
1. Open PuTTYgen
2. Click "Load"
3. Change file filter to "All Files (*.*)"
4. Select your .pem file
5. Click "Save private key"
6. Save as .ppk file

### Server Connection Setup
1. WinSCP Configuration:
   - File Protocol: SFTP
   - Host: [EC2 Public DNS]
   - Port: 22
   - Username: ec2-user
   - Private key file: Browse to .ppk
   
2. PuTTY Configuration:
   - Host: [EC2 Public DNS]
   - Port: 22
   - Connection > SSH > Auth > Credentials
   - Browse for .ppk file
   - Save session (optional)

## Deployment Process

### 1. Install Web Server
1. Connect via PuTTY as ec2-user
2. Run commands:
   ```bash
   sudo yum install httpd
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

### 2. File Upload
1. Connect via WinSCP
2. Navigate to /var/www/html
3. Upload website files
4. Set permissions:
   ```bash
   sudo chmod -R 755 /var/www/html
   sudo chown -R ec2-user:ec2-user /var/www/html
   ```

### 3. Domain Configuration
1. Point domain to EC2 public IP
2. Add DNS records:
   - A record: @ → [EC2 IP]
   - CNAME: www → [domain]

## Common Issues and Solutions

### 1. Missing /var/www/html Directory
**Problem:** Directory not found
**Solution:** Install Apache:
```bash
sudo yum install httpd
sudo systemctl start httpd
```

### 2. Permission Denied Errors
**Problem:** Cannot upload files
**Solutions:**
1. Basic fix:
   ```bash
   sudo chmod -R 755 /var/www/html
   sudo chown -R ec2-user:ec2-user /var/www/html
   ```
2. Advanced fix (if still having issues):
   ```bash
   sudo su -
   chmod -R 777 /var/www/html
   ```

### 3. Cannot Login as Root
**Problem:** Direct root login denied
**Solution:** Use ec2-user with sudo:
```bash
sudo [command]
```

## Testing
1. Test via EC2 URL:
   - http://[EC2-Public-DNS]/
2. Test via domain:
   - http://yourdomain.com
   - http://www.yourdomain.com

## Troubleshooting Checklist
- Apache installed and running
- Correct file permissions
- DNS propagation complete
- Security group allows port 80
- Files in correct directory
