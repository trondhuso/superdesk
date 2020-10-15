# Superdesk
[![Build Status](https://travis-ci.com/superdesk/superdesk.svg?branch=master)](https://travis-ci.com/superdesk/superdesk)
[![Coverage Status](https://coveralls.io/repos/superdesk/superdesk/badge.svg)](https://coveralls.io/r/superdesk/superdesk)
[![Code Climate](https://codeclimate.com/github/superdesk/superdesk/badges/gpa.svg)](https://codeclimate.com/github/superdesk/superdesk)
[![Requirements Status](https://requires.io/github/superdesk/superdesk/requirements.svg?branch=master)](https://requires.io/github/superdesk/superdesk/requirements/?branch=master)

Superdesk is an open source end-to-end news creation, production, curation,
distribution and publishing platform developed and maintained by Sourcefabric
with the sole purpose of making the best possible software for journalism. It
is scaleable to suit news organizations of any size. See the [Superdesk website](https://www.superdesk.org) for more information.

Looking to stay up to date on the latest news? [Subscribe](http://eepurl.com/bClQlD) to our monthly newsletter.

The Superdesk server provides the API to process all client requests. The client
provides the user interface. Server and client are separate applications using
different technologies.

Find more information about the client configuration in the README file of the repo:
[github.com/superdesk/superdesk-client-core](https://github.com/superdesk/superdesk-client-core)

## Installation on fresh Ubuntu 16.04

```sh
curl -s https://raw.githubusercontent.com/superdesk/fireq/files/superdesk/install | sudo bash
# Open http://<ip_or_domain> in browser
# login: admin
# password: admin
```

More options and details:
- [for users](https://github.com/superdesk/fireq/tree/files/superdesk)
- [for developers](https://github.com/superdesk/fireq/tree/files/superdesk#development)

## Manual installation

### Requirements

These services must be installed, configured and running:

 * MongoDB
```sh
   wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
   apt update
   apt install -y mongodb-org

   # start mongodb
   systemctl start mongodb.service

   # control your installation with
   systemctl status mongodb.service

   # if it says active(running) run the following command
   systemctl enable mongodb.service 
```

 * ElasticSearch (7.x)
```sh
   curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
   apt update
   apt install elasticsearch

   # correct config-file for elasticsearch
   cd /etc/elasticsearch
   vi elasticsearch.yml

   # mark out or change network.host and network.port
   # I have set it to localhost, but you can also use the IP of your server
   network.host: localhost
   network.port: 9200

   # correct jvm.options
   # if you are on a small dev machine /server with only 2gb ram
   -Xms512m
   -Xmx512m

   # start elasticsearch
   systemctl start elasticsearch.service
  
   # control status
   systemctl status elasticsearch.service

   # if status is active(running)
   systemctl enable elasticsearch.service
```

 * Redis
```sh
   apt install -y redis-server

   # change one setting in redis.conf
   # supervised no
   # change to
   supervised systemd

   # restart redis
   systemctl restart redis.service 

   # check status
   systemctl status redis.service 
   
   # if running set it to
   systemctl enable redis.service
```

 * Python (>= 3.6)
```sh
   # check if python 3 is installed
   python3 -V

   apt install -y python3-pip
```

 * Node.js (with `npm`)
```sh
   # installing node and npm
   install -y nodejs npm
```

On macOS, if you have [homebrew](https://brew.sh/) installed, simply run: `brew install mongodb elasticsearch redis python3 node`.

### Installation steps:

```sh
path=~/superdesk
git clone https://github.com/superdesk/superdesk.git $path

# server
cd $path/server
pip3 install -r requirements.txt
python3 manage.py app:initialize_data

# change this to your standard e-mail and username
python3 manage.py users:create -u admin -p admin -e 'admin@example.com' --admin
honcho start
# if you need some data
python manage.py app:prepopulate

# client
cd $path/client
npm install
grunt server

# open http://localhost:9000 in browser
```

#### :warning:  macOS users

All the above commands need to run inside the Python Virtual Environment, which you can create
using the `pyvenv` command:

- Run `pyvenv ~/pyvenv` to create the files needed to start an environment in the directory `~/pyvenv`.
- Run `. ~/pyvenv/bin/activate` to start the virtual environment in the current terminal session.

Now you may run the installation steps from above.

### Questions and issues

- Our [issue tracker](https://dev.sourcefabric.org/projects/SD) is only for bug reports and feature requests.
- Anything else, such as questions or general feedback, should be posted in the [forum](https://forum.sourcefabric.org/categories/superdesk-dev).

### A special thanks to...

Users, developers and development partners that have contributed to the Superdesk project. Also, to all the other amazing open-source projects that make Superdesk possible!

### License

Superdesk is available under the [AGPL version 3](https://www.gnu.org/licenses/agpl-3.0.html) open source license.
