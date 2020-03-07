# JS Server App Setup Example

Created this example to be used to test that PM2 is setup correctly when creating new ubuntu servers for hosting Node.js applications.

## Install software
- Firstly update the server
```
sudo apt -y update
```

### Nginx
- Install nginx
```
sudo apt install -y nginx
```
- Check that nginx is running, you should see "Active: active (running)" in the output
```
sudo service nginx status
```
- Press Ctrl + C to quit status
### NodeJS
- Check nodejs website to verify which version of nodejs you want to install
[NodeJS.org](https://nodejs.org/en/)
- Install nodejs
```
curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -
sudo apt-get install -y nodejs
```
### PM2
- Install PM2 via NPM
```
sudo npm install pm2@latest -g
```
## Setup helloworld example
### Add PM2 host
- Create ecosystem.config.js file to store hosts, I use vim as a text editor inside of ubuntu
```
vim ~/ecosystem.config.js
```
```
module.exports = {
  apps : [{
    name: 'helloworld',
    cwd: '/srv/nodejs-helloworld',
    script: 'node',
    args: 'index.js'
  }]
};
```
- Add the helloworld app to PM2 using the ecosystem.config.js with the following command
```
pm2 start ~/ecosystem.config.js
```
- Test that the helloworld example is running
```
curl 127.0.0.1:3000
Hello World! This is NodeJS running
```
- Setup PM2 to load the host when the serve restarts
```
pm2 save
pm2 startup
```
- After running the startup command you will see a command output, copy and run the command to add the systemd entry to enable PM2 to run on server boot
```
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu
```
### Add nginx host

- Go to the nginx vhost directory and use vim to create a new vhost file
```
cd /etc/nginx/sites-available
sudo vim helloworld
```
- Use the following example for the content of the vhost
	- Change server_name to website address that points to your server IP address
```
server {
    listen 80;

    server_name meshu.site;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
- Add the site host to the enabled sites-enabled directory
```
sudo ln -s /etc/nginx/sites-available/helloworld /etc/nginx/sites-enabled/helloworld
```
- Reload the nginx server to apply changes
```
sudo service nginx reload
```
- Go to website address in your browser and the nodejs app should load
[http://meshu.site](http://meshu.site)
