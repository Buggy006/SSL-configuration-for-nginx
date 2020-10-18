# Setup SSL for Nginx
#### The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 1 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below: Install and configure nginx on App Server 1. On App Server 1 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx. Create an index.html file with content Welcome! under Nginx document root. For final testing try to access the App Server 1 link (either hostname or IP) from jump host using curl command. For example curl -Ik https:// <app-server-ip>/.

#### Solution:-
##### SSh on App Server According to Task

```
ssh tony@stapp01
sudo  yum install epel-release -y && sudo yum install  -y nginx && sudo vi /etc/nginx/nginx.conf
````
#### Place Below commands in nginx.conf file as Shown in a image 
![image](https://lh5.googleusercontent.com/1w12aXqUd2SmwlTpZtS0SMZYATznYrIS5QBwQDiOQtTkdpT3Ol7e64rawS-C8D24lLyww_Gv2VDTT0xEYL0xqd5tZJcnN5fnAinrlx0T)
```
ssl_certificate  "/etc/pki/CA/certs/nautilus.crt";
ssl_certificate_key  "/etc/pki/CA/private/nautilus.key";
ssl_session_cache shared:SSL:1m;
ssl_session_timeout    10m;
ssl_ciphers  HIGH:!aNULL:!MD5;
ssl_prefer_server_ciphers on;
```
#### save the file and restart the Service
```
sudo cp /tmp/nautilus.crt /etc/pki/CA/certs && sudo cp /tmp/nautilus.key  /etc/pki/CA/private
sudo systemctl restart nginx && sudo systemctl enable nginx
sudo echo  Welcome!>>index.html
sudo mv index.html /usr/share/nginx/html
sudo yum install curl -y
curl -Ik https://172.16.238.10  ## use ip of app server
```
