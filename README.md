# Monitor your Mikrotik router with Prometheus and Grafana

Over the past years I replaced all my networking gear with Mikrotik devices. Nothing compares to Mikrotik in terms of price, features, performance and reliability. I absolutely love WinBox for management, but sometimes I miss some fancy charts and graphs. Luckily, RouterOS comes with a REST-API that can be used to query arbitrary data from the device.

## Setup

- Router running RouterOS 7.x.x
- Raspberry Pi 4 with 2 gb RAM

## Demo pictures

![General system stats](https://github.com/M0r13n/mikrotik_monitoring/blob/main/doc/pic1.png)
![Wifi stats](https://github.com/M0r13n/mikrotik_monitoring/blob/main/doc/pic2.png)

## Installation

## Mikrotik Router
At first you need to prepare your router. 

Create a group on the device that has API and read-only access:

`/user group add name=prometheus policy=api,read,winbox,test`

Create a user that is part of the group:

`/user add name=prometheus group=prometheus password=TOP_SECRET`

Because the library makes a new connection for every API request, your logs are getting cluttered:

`system logging set 0 topics=info,!account`

## Prepare Raspi

Install Python and pip:

`sudo apt install python3-dev python3 python3-pip -y`

Install Docker + Docker-compose

```bash
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker ubuntu
sudo pip3 install docker-compose
sudo systemctl enable docker
```

Spin up Grafana and Prometheus:

```bash
# Clone this repo
git clone https://github.com/M0r13n/mikrotik_monitoring.git

# Go into the cloned directory
cd mikrotik_monitoring

# Let docker-compose do it's job
sudo docker-compose up -d
```

Done. You should now be able to open the Grafana dashboard on Port 3000 of your Raspberry Pi.
