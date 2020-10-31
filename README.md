# Running server in 5 minutes :zap:

## Create the project 💥

### Initialize the project
```bash
npm init -y
```

### Install express.js
```bash
npm install express
```

### Create `index.js`
```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => {
	res.send('<h1>Hello! 😁</h1>');
})

app.listen(3000, () => console.log('🚀🚀') );
```

### Modify npm scripts in `package.json`
```json
// ...
"scripts": {
    "start": "node index.js",
}
// ...
```

## Dockerize the app 🐳

### Create the Dockerfile
```docker
FROM node

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY index.js ./
EXPOSE 3000
CMD ["npm", "start"]
```

### Build the image
```bash
docker build --tag five_minute_server .
```

### Test the image
```bash
docker run --publish 3000:3000 --name fms five_minute_server
```

```bash
docker stop fms
```

## Export your app 🚀

### Create a droplet at Digital Ocean
- Sign In / Sign Up at Digital Ocean
- Click in `Droplets`, in the left tab
- Click in `Create Droplet`
- Select the cheaper droplet
- Select password authentication for a fast setup (although SSH keys are preferred for a longer project)
- Create the droplet

You will get an IP `<ip>` (e.g. `157.230.239.91`)

### Copy the files to the droplet

Create a tar file

```bash
tar -czf server.tar.gz  Dockerfile index.js package.json package-lock.json
```

Send the tar file to the droplet (with IP `<ip>`)

```bash
scp server.tar.gz root@<ip>:/root
```

### Setup the server

Connect to the droplet

```bash
ssh root@<ip>
```

Unpack the tar file

```bash
tar -xzf server.tar.gz
```

Install docker (based on [docker installation tutorial on Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04))


```bash
apt update

apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

apt update

apt-cache policy docker-ce

apt install docker-ce
```

Restart the session on the droplet

```bash
ssh root@<ip>
```

Build the image

```bash
docker build --tag five_minute_server .
```

### Run the server

```bash
docker run --detach --publish 3000:3000 --name fms five_minute_server 
```

### Access the server on your browser

The address will be `<ip>:3000`

## And you're done 🎉

How have now a dockerized Node.js server running in a remote machine and accessible by anyone.

