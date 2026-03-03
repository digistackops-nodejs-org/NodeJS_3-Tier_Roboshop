# Frontend Setup
### Launch EC2 "t2.micro" Instance and In Sg, Open port "80" for nginx
# Step:1 ==> Install the Required packages
#### Install Nginx
```
sudo yum install nginx -y
```
Start the Service
```
sudo systemctl start nginx
sudo systemctl enable nginx
```
Remove the default content that web server is serving.
```
rm -rf /usr/share/nginx/html/* 
```

# Step:2 ==> Get the Code
We keep application in one standard location. This is a usual practice that runs in the organization. Lets setup an app directory.
```
sudo mkdir -p /app
```
To start this application first you can get the code using below url
Clone the Repo
```
cd /app
sudo git clone https://github.com/digistackops-nodejs-org/NodeJS_3-Tier_Roboshop.git
cd NodeJS_3-Tier_Roboshop
```
Switch to Local-setup Branch
```
sudo git checkout 01-Local-setup-Prod
```

# Step:3 ==> Run the Application
Move the frontend content to the Nginx Folder
```
cd /app/NodeJS_3-Tier_Roboshop/frontend 		 
cp *  /usr/share/nginx/html
```
Create Nginx Reverse Proxy Configuration.
```
vim /etc/nginx/default.d/roboshop.conf 
```
Add these Lines 
```
proxy_http_version 1.1;
location /images/ {
  expires 5s;
  root   /usr/share/nginx/html;
  try_files $uri /images/placeholder.jpg;
}
location /api/catalogue/ { proxy_pass http://<catalogue-IP>:8080/; }  # Add you catalogue server private-IP

location /health {
  stub_status on;
  access_log off;
}
```
Restart Nginx Service to load the changes of the configuration.
```
sudo systemctl restart nginx 
```
