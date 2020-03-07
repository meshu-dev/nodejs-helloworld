# NodeJS Helloworld Example

Created this example to be used to test that PM2 is setup correctly when creating new ubuntu servers for hosting nodejs applications.

## Install software
- Firstly update the server
```
sudo apt update
```
- Check nodejs website to verify which version of nodejs you want to install
[NodeJS.org](https://nodejs.org/en/)

### Nginx
- Install nginx

### NodeJS
```
sudo apt install -y nginx
```
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
- Create ecosystem.config.js file to store hosts
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
