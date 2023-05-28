# Setting Up Elasticsearch Master and data nodes.

## Requirements:

- Minimum specs: 2 core CPU, 4GB RAM. (t2.medium in this demo)
- Minimum 2 VMs (or EC2 instances in this demo)
- Operating system: any supported OS, refer [official docs](https://www.elastic.co/guide/en/elasticsearch/reference/8.6/install-elasticsearch.html) for more info. Ubuntu 22.04 is used for this demo.

## Steps:

- Let's start by changing hostnames of the VMs for easy understanding and efficiency. Do same for both master and data node virtual machine.

  ```bash
  sudo hostnamectl set-hostname <desired name>
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-00-17.png)

  exit the VM and connect again so that changes will take effect.

- Install the **elastic search** in both master and data node machines. (Please follow [official docs](https://www.elastic.co/guide/en/elasticsearch/reference/8.6/install-elasticsearch.html) for other OS)

  ```bash
  wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.6.2-amd64.deb

  wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.6.2-amd64.deb.sha512

  shasum -a 512 -c elasticsearch-8.6.2-amd64.deb.sha512

  sudo dpkg -i elasticsearch-8.6.2-amd64.deb
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-03-50.png)
  ![](__assets__/Screenshot%20from%202023-03-11%2015-04-53.png)
  ![](__assets__/Screenshot%20from%202023-03-11%2015-05-46.png)

- After installation at the end you will see security configuration information (as shown in screenshot above) save the entire block to somewhere.

- Now edit the configuration file located at `/etc/elasticsearch/elasticsearch.yml` (\*note: different OS and linux distributions may have different configuration paths.)
  add following parameters. **\*Only for master node**

  ![](__assets__/Screenshot%20from%202023-03-11%2015-17-12.png)
  ![](__assets__/Screenshot%20from%202023-03-11%2015-17-35.png)

- Enable following ports of Master node machine: 9200, 9300 and 5601.

- Reload the daemon and enable/start the elasticsearch service and the elasticearch service should start.

  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable elasticsearch.service --now
  sudo systemctl status elasticsearch.service
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-23-58.png)

- If everything is installed and running correctly you should see similar output in data node machine.

  ```bash
  curl https://master-node-ip:9200
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-25-08.png)

- Now generate the enrollment token for data node with following command and save it somewhere.

  ```bash
  cd /usr/share/elasticsearch/bin
  sudo ./elasticsearch-create-enrollment-token -s node
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-52-28.png)

- Now edit the same file in **data node VM** and add following parameters. **\*Only for data node**
  ![](__assets__/Screenshot%20from%202023-03-11%2015-33-35.png)

- Create superuser in **both master and data node**.

  ```bash
  cd /usr/share/elasticsearch/bin
  sudo ./elasticsearch-users useradd <username> -r superuser
  ```

- Now add the node to the existing cluster with the enrollment token we generated before. Run the following command on the **data node machine**.

  ```bash
  cd /usr/share/elasticsearch/bin
  sudo ./elasticsearch-reconfigure-node --enrollment-token <token-here>
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-53-05.png)

- Reload the daemon and enable/start the elasticsearch service and the elasticearch service should start.

  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable elasticsearch.service --now
  sudo systemctl status elasticsearch.service
  ```

- After the service started successfully, check whether the data node is added to the cluster or not. Run following command on **master-node machine**.

  ```bash
  curl --cacert /etc/elasticsearch/certs/http_ca.crt -X GET "https://localhost:9200/_cat/nodes?v=true&pretty" -u <elasticsearch-user-we-created>
  ```

  ![](__assets__/Screenshot%20from%202023-03-11%2015-54-35.png)

- We have successfully created an elasticsearch cluster with master and data node.

## If We want to add another **master-eligible** node to create a multi-master stack then follow below steps.

- Consider using same SSL cert for transport. We can generate said certificate by following [offical doc](https://www.elastic.co/guide/en/elasticsearch/reference/8.6/security-basic-setup.html#encrypt-internode-communication).
- Basically setup is same except for 1-2 added steps.
- Generate cert using utility provided by elastic. \*Note: You have to execute following command in master-1 node (and only once, since we will be using this one generated cert everywhere.)
  ```bash
  ./bin/elasticsearch-certutil ca
  ```
- And after generating CA we have to generate a certificate and private key for each (considering that you have installed elasticsearch on master-2 and haven't configured it yet!) of the nodes in your cluster (including master-1). You include the elastic-stack-ca.p12 output file that you generated in the previous step.
  ```bash
  ./bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12
  ```
- After generating cert (p12 file) consider adding them to a easy-to-remember location (used "/etc/elasticsearch" here).

- Its time to add them into configurations with other changes.

- Master-1 configuration (/etc/elasicsearch/elasicsearch.yml).
  ![](__assets__/Screenshot%20from%202023-05-28%2020-31-57.png)
  ![](__assets__/Screenshot%20from%202023-05-28%2020-32-16.png)

- For Master-2 (Donot forget to add same cluster name).
  ![](__assets__/Screenshot%20from%202023-05-28%2020-44-28.png)
  ![](__assets__/Screenshot%20from%202023-05-28%2020-44-52.png)

- And then restart msater-1 node and master-2 node.
