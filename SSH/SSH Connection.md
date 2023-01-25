
* ## Normal Method (Password Authentication)

	 ### Command:

	*# ssh username@ip_addrdess -p <port_no.>*

Here, port number is optional. By default port 22 is used; If we have configured the port in */etc/ssh/sshd_config* file then, we have to mention the port number after flag *-p*.

![](Attachments/Pasted%20image%2020221227203757.png)

*Port based:*
![](Attachments/Pasted%20image%2020221227203932.png)

* ## Passwordless Authentication (Key Authentication)

	 ### Setting up the keys
	 
	 ### Command:
	 To Generate the keys:
	 *# ssh-keygen*
	 ![](Attachments/Pasted%20image%2020221227204203.png)

	 Now share the public key to the server.
	 
	 *# ssh-copy-id -i < path to the .pub file > username@server_ip*
	 ![](Attachments/Pasted%20image%2020221227204922.png)

	 After this step, verify the passwordless authentication by establishing ssh connection.
	 
	 *# ssh username@server_ip -i < path to the private key >*
	 ![](Attachments/Pasted%20image%2020221227205153.png)
