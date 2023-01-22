# Implementing File transfer between two VMs in Azure.

## Requirements:

- Azure Account.
- Virtual Network with two subnets- Private Subnet and Puvlic Subnet.
- One Jump server (In public subnet with Public IP).
- Two VMs connected to a private subnet (Without any Public IP).
- Azure Storage Account.

## Steps:

- Search and select **file share** in azure storage account.

  ![](Assets/Screenshot%20from%202023-01-21%2020-28-43.png)

- After Creating a file share login into both VMs and install packages with following links.

  ```bash
  sudo apt update
  sudo apt install cifs-utils
  ```

  ![](Assets/Screenshot%20from%202023-01-21%2021-27-31.png)

* Then go to overview tab in storage account.

  ![](Assets/Screenshot%20from%202023-01-21%2021-28-13.png)

* Now add the given bash script code in a script file in both of the VMs.

  ![](Assets/Screenshot%20from%202023-01-21%2021-28-46.png)

* Give execute permissions.

  ```bash
  chmod u+x <script-name>
  ```

* Execute the script in both of the VMs.

  ```bash
  ./<script-name>
  ```

* Now access the connected file share in **/mnt** folder.

  ![](Assets/Screenshot%20from%202023-01-21%2021-32-40.png)

  ![](Assets/Screenshot%20from%202023-01-21%2021-33-33.png)

  ![](Assets/Screenshot%20from%202023-01-21%2021-34-04.png)

  ![](Assets/Screenshot%20from%202023-01-21%2021-34-49.png)

* Check if the added/shared files between VMs reflect on the Azure portal or not.

  ![](Assets/Screenshot%20from%202023-01-21%2021-35-12.png)
