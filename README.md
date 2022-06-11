# Aker-Systems-Project-3-MERN-Stack-Implementation

## Intro

In this project I'm going to implement a MERN stack web application consisting off the following:
- MongoDB  -- The database
- ExpressJs -- backend
- ReactJS -- backend
- Node.js -- front-end/client-side

This will be done using an aws EC2.

## Backend Configuration

Firstly I run the series of command below to update and upgrade my OS, along with installing NodeJS, and verifying the installation.

```
# Ubuntu (20.04) ec2
sudo apt update
sudo apt upgrade

# get location of node.js from ubuntu repository, amd install
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# Check installation
sudo apt-get install -y nodejs
node -v 
npm -v
``

![alt text](/node.png)