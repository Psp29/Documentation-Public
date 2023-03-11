# Setting Up Filebeat to get logs directly from a webserver.

## Requirements:

- Eveything that was specified in [previous doc](./02_Kibana_Setup.md).
- A Nginx Webserver.

## Steps:

- We will be setting up filebeat on the **Nginx Webserver**. Please refer [official filebeat docs from elastic](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html) for OS specific instructions on how to download and install the Filebeat package.

- For ubuntu 22.04 run following commands.

  ```bash
  curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.6.2-amd64.deb
  sudo dpkg -i filebeat-8.6.2-amd64.deb
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-40-18.png)
  ![](__assets__/Screenshot%20from%202023-03-11%2016-41-04.png)

- We have to get the CA Fingerprint of the elasticsearch master certificate. To get that first run the following command on the **elasticsearch master node machine**.

  ```bash
  cd /etc/elasticsearch/certs/
  openssl x509 -fingerprint -sha256 -in http_ca.crt
  ```

  It will give result as shown below.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-46-22.png)
  **convert the aquired fingerprint to all lowercase and remove all the delimiters (colons ':') and save it somewhere for later use.**

- Now for accessing the passwords and secrets securely filebeat provides keystore utility (read more [here](https://www.elastic.co/guide/en/beats/filebeat/8.6/keystore.html)).

  ```bash
  sudo filebeat keystore add ES_PWD
  #add your elasticuser password when prompted.

  sudo ES_CA
  #add CA fingerprint that saved before when prompted.
  ```

- Next configure filebeat itself by editing `filebeat.yml`.

  ```bash
  cd /etc/filebeat
  sudo nano filebeat.yml
  ```

  add Elasticsearch Machine URL (VM's public URL if performing on cloud VMs) in kibana section as shown below.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-50-40.png)

  now edit the elasticsearch section. add Elasticsearch Machine URL (VM's public URL if performing on cloud VMs) and other important parameters as shown below.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-53-05.png)
  after that save the `filebeat.yml` file.

- To check the available filebeat modules.

  ```bash
  sudo filebeat modules list # It will list all available modules

  sudo filebeat modules list | grep "nginx" # It will list specific available modules ('nginx' in this case)
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-54-08.png)

- To enable a module.

  ```bash
  sudo filebeat modules enable nginx
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-54-27.png)

- Edit the module-specific configuration file (`nginx.yml` in this case)

  ```bash
  sudo nano /etc/filebeat/modules.d/nginx.yml
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2016-55-18.png)
  add following parameters.
  ![](__assets__/Screenshot%20from%202023-03-11%2016-56-09.png)

- Filebeat comes with predefined assets for parsing, indexing, and visualizing your data. To load these assets run following command

  ```bash
  sudo filebeat setup -e
  ```

  upon successful run it will yeild following output.
  ![](__assets__/Screenshot%20from%202023-03-11%2017-16-48.png)

- Enable and start the filebeat service.

  ```bash
  sudo systemctl enable filebeat --now
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2017-17-43.png)

- Now head over to kibana dashboard and refres the page, login again -> hamburger-menu -> dashboard
  ![](__assets__/Screenshot%20from%202023-03-11%2017-18-04.png)

- Search for nginx in dashboards.
  ![](__assets__/Screenshot%20from%202023-03-11%2017-18-29.png)

- Select access and error logs and it will show the timeline and logs.
  ![](__assets__/Screenshot%20from%202023-03-11%2017-19-22.png)

- The Filebeat setup is Complete.
