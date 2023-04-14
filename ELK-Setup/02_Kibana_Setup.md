# Setting Up Kibana.

## Requirements:

- Eveything that was specified in [previous doc](./01_Elasticsearch_Setup.md).

## Steps:

- We will be setting up kibana on the **master-node Virtual Machine**. Please refer [official kibana docs from elastic](https://www.elastic.co/guide/en/kibana/8.6/install.html) for OS specific instructions on how to download and install the kibana package.

- On ubuntu install the kibana with the help of following commands.

  ```bash
  wget https://artifacts.elastic.co/downloads/kibana/kibana-8.6.2-amd64.deb
  sudo dpkg -i kibana-8.6.2-amd64.deb
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-55-48.png)
  ![](__assets__/Screenshot%20from%202023-03-11%2015-56-59.png)

- Generate token for kibana by running following commands. Also save the kibana token for later use.

  ```bash
  cd /usr/share/elasticsearch/bin
  sudo ./elasticsearch-create-enrollment-token -s kibana
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-58-53.png)

- Reload the daemon and enable/start the kibana service and the kibana service should start.

  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable kibana.service --now
  sudo systemctl status kibana.service
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-59-58.png)

- Configure Reverse Proxy with nginx. Install nginx and configure nginx as shown below.

  ```bash
  sudo apt update
  sudo apt install nginx
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-01-01.png)

  disable the default webpage and config file.

  ```bash
  cd /etc/nginx/sites-enabled
  sudo unlink default
  ll
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-01-39.png)

  Create new config file for kibana.

  ```bash
  sudo nano ../sites-available/kibana.conf
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-04-18.png)

  add following configuration in kibana.conf

  ```bash
  upstream elasticsearch {

    server 127.0.0.1:9200;
    keepalive 15;
  }

  upstream kibana {

    server 127.0.0.1:5601;
    keepalive 15;
  }

  # To solve 413 error.
  client_max_body_size 100M;

  server {

    listen 7469;

    location / {

  #    auth_basic "Restricted Access";
  #    auth_basic_user_file /etc/nginx/htpasswd.users;

      proxy_pass http://kibana;
      proxy_redirect off;
      proxy_buffering off;

      proxy_http_version 1.1;
      proxy_set_header Connection "Keep-Alive";
      proxy_set_header Proxy-Connection "Keep-Alive";
    }
  }
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-06-18.png)

- Now create a soft link of the nginx conf file (kibana.conf) to `/etc/nginx/sites-enabled`

  ```bash
  sudo ln -s /etc/nginx/sites-available/kibana.conf /etc/nginx/sites-enabled
  ll
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-08-25.png)

- Check the nginx config syntax and restart nginx service.

  ```bash
  sudo nginx -t
  sudo systemctl restart nginx
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-08-42.png)

- To check whether we can access the kibana on our browser, hit the public IP or private IP (depending upon you are performing on local machine or a cloud VM) on your browser.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-09-53.png)

- First it will ask for the **kibana token**, put the token that we already generated and saved. After adding token it will ask you to put a **6 digit code**; generate the code by running the command it shows. After that it will ask you to login, use the **elasticsearch credentials**. Upon successful login kibana will greet you with following screen.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-12-06.png)

### To make the master-node the **master-only** node (as by default the master node is also behaves as the data node and has other roles) follow next steps.

- First stop the running elasticsearch service.

  ```bash
  sudo systemctl stop elasticsearch.service
  ```

- Edit elasticsearch configuration file `elasticsearch.yml`

  ```bash
  sudo nano /etc/elasticsearch/elasticsearch.yml
  ```

- Add `node.roles: [master]` line as shown below.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-13-03.png)

- Now run following command.

  ```bash
  cd /usr/share/elasticsearch/bin
  ./elasticsearch-node repurpose
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-16-24.png)

- Restart elasticsearch and kibana respectively.

  ```bash
  sudo systemctl restart elasticsearch
  sudo systemctl status elasticsearch
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-19-24.png)

  ```bash
  sudo systemctl restart kibana
  sudo systemctl status kibana
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-19-43.png)

- To run the elasticsearch API calls without using curl, click hamburger-menu -> dev-tools -> console

  ```bash
  GET _cat/nodes
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-23-20.png)

  to get same from terminal with curl command.

  ```bash
  curl --cacert /etc/elasticsearch/certs/http_ca.crt -X GET "https://localhost:9200/_cat/nodes?v=true&pretty" -u <elasticsearch-username>
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-23-41.png)

- And we are done with the Kibana Setup.
