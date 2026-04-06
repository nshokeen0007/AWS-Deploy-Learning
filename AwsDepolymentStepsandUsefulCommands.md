
-------------- AWS DEPLOYMENT ---------------------

# Permission --
sudo chown -R www-data:www-data /var/www/frontend
sudo chmod -R 755 /var/www/frontend





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













