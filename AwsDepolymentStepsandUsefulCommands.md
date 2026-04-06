
-------------- AWS DEPLOYMENT ---------------------

# Permission --
sudo chown -R www-data:www-data /var/www/frontend
sudo chmod -R 755 /var/www/frontend


sudo mkdir -p /var/www/frontend --making folder 
sudo cp -r /home/ubuntu/frontend/build /var/www/frontend/quickconnect-console   #here we copy that one location to another location


pm2 list 
sudo pm2 restart app.js







# New setup Commands

After downloading the pem file..
step 1.

---->chmod 400 "filename.pem"


step 2. login with that pem file

--->ssh -i "filename.pem" ubuntu@ipaddress

note :- enter your filename and ipadress here 

step 3:then update your system and package

---->sudo apt update && sudo apt upgrade -y

step 4: install nginx and unzip(if you want to dont use WinSCP)

---->sudo apt install nginx -y
---->sudo apt install unzip -y

note:-apt stands for adavance package tool.

step 5:install nodejs 

--->curl -fsSL https://nodesource.com | sudo -E bash -
--->sudo apt install nodejs -y

note: if you want to check that version 

---> node -v
---> npm -v

step 6: Install pm2 for nodejs

---> sudo npm install -g pm2


step 7 :- If you deploy manually then..

RUN in local cmd not in serve

$ scp -i "learning.pem" -r "C:\Users\xyz\Desktop\learning_react\client\vite-project\dist" ubuntu@ipadress:/var/www/frontend/xyz-projectname/



note:-if react app not vite then take build..

and also backend 


$ scp -i "learning.pem" -r "C:\Users\xyz\Desktop\learning_react\client\vite-project\backend.zip" ubuntu@ipadress:/var/www/backend/xyz-projectname/


note if some permission error occur then run(same for backend folder)

sudo chown -R www-data:www-data /var/www/frontend
sudo chmod -R 755 /var/www/frontend



# After push your porject

------For frontend -------

step 1 -- create that config file on sites-available

sudo nano /etc/nginx/sites-available/xyz-project

note:-nano is a editor 

step 2

server {
    listen 80;
    server_name _;   

    #server_name quickconnectconsole.randomschat.fun;
    # if uh have any domain the remove that _ and put that domain for frontend here example...quickconnectconsole.randomschat.fun

    # React frontend
    root /var/www/frontend/xyz-project/build;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    # Backend API
    location /api/ {
        proxy_pass YourbackendIp:PORT OR domain/; #BACKENDIP:PORT
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}



step 3 : after edit that config  put them in sites-enabled

sudo ln -s /etc/nginx/sites-available/quickconnect-frontend /etc/nginx/sites-enabled/


step 4: restart nginx

sudo systemctl restart nginx


----note for checking that nginx is running fine 
sudo nginx -T


step 5 : SSL CERTIFICATE 

sudo apt install certbot python3-certbot-nginx -y #only for ones
sudo certbot --nginx -d quickconnectconsole.randomschat.fun 

note :quickconnectconsole.randomschat.fun is my domain---change this 



-----------------For backend --------------config

Step 1: Connect Domain to Server IP (DNS Settings)

Log in to your Domain Provider dashboard (e.g., GoDaddy, Namecheap).

Go to the DNS Management or Advanced DNS section.

Add a new A Record:

Type: AHost: @ (this represents our main domain)Value/Points to: 1.1.1.2.31 (Your Server IP)(Optional) 
Add another A Record:
Type: AHost: wwwValue: 1.314.134.134(Our server ip)

Step 2: Update Nginx ConfigurationOpen our Nginx config file on the server:sudo nano /etc/nginx/sites-available/defaultUpdate the server_name line with your domain:nginxserver {
    listen 80;
    server_name yourdomain.com ://yourdomain.com;

    location / {
        proxy_pass http://localhost:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
Use code with caution.
Save the file (Ctrl+O, Enter, then Ctrl+X).
Test and restart
 Nginx:
 
 sudo nginx -t (Checks for errors)
 sudo systemctl restart nginx
 
 Step 3: Install SSL (HTTPS) via CertbotInstall the Certbot software:
 
 sudo apt updatesudo apt install certbot python3-certbot-nginx
 Generate the SSL certificate
 sudo certbot --nginx -d yourdomain.com -d ://yourdomain.com











