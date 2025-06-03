1. Launch an EC2 Instance
   -> Go to AWS Console → EC2 → Launch Instance.
   -> Choose Ubuntu (or Amazon Linux) as OS.
   -> Select instance type (t2.micro for testing).
   -> Set up key pair for SSH.
   -> Allow ports 22 (SSH), 80 (HTTP), and 443 (HTTPS) in security group.

2. SSH into Your EC2 Instance
   -> ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip

3. Install Node.js and npm

  -> sudo apt update (Updates the list of available packages and versions from the repositories.It does not install anything, just refreshes the catalog., ubuntu machine packages)
  -> sudo apt install nodejs npm -y
  -> node -v
  -> npm -v

 4. clone from GitHub:
   -> git clone <your-repo-url>

5. Install Project Dependencies
  -> cd /home/ubuntu/your-project-folder 
  -> npm install

6. Start Your Node.js App (with PM2 for production)
  -> sudo npm install -g pm2
  -> pm2 start app.js   # or your main file
  -> pm2 startup
  -> pm2 save

7. Install Nginx
  -> sudo apt install nginx -y

8. Configure Nginx as a Reverse Proxy
  -> Edit Nginx config:
       -> sudo nano /etc/nginx/sites-available/default
       -> Replace the server block with:
      -> server {
                listen 80;
                server_name your_domain_or_ip;
            
                location / {
                    proxy_pass http://localhost:3000; # or your Node.js port
                    proxy_http_version 1.1;
                    proxy_set_header Upgrade $http_upgrade;
                    proxy_set_header Connection 'upgrade';
                    proxy_set_header Host $host;
                    proxy_cache_bypass $http_upgrade;
                }
            }
      -> Save and exit (Ctrl+O, Enter, Ctrl+X).
   -> Test and reload Nginx: 
         -> sudo nginx -t
         -> sudo systemctl reload nginx

9. Get SSL Certificate with Certbot (Let's Encrypt)
  -> sudo apt install certbot python3-certbot-nginx -y (Install Certbot to manage SSL certificates:)
  -> sudo certbot --nginx -d your-domain.com (Obtain SSL certificate for your domain:)

10. Check your existing SSL certificates:
  -> sudo certbot certificates
  -> sudo certbot renew (To renew all domains' certificates:)
  -> sudo systemctl reload nginx (reload , for stop use stop)

