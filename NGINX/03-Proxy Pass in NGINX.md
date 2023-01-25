Proxying is typically used to distribute the load among several servers, seamlessly show content from different websites, or pass requests for processing to application servers over protocols other than HTTP.
In real life scenario the reverse proxy can be used mainly for two reasons, one being security and other is load balancing.

![](Attachments/reverse%20proxy%201.png)

* Note: Nginx works as reverse proxy rather than forward proxy.

## Setting up NGINX as a Reverse-Proxy

### Prerequisites:
* Linux Server/VM
* Nginx as Web-host.
* Content files (e.g. Html, CSS, JS files).

![](Attachments/Pasted%20image%2020230102160343.png)
Here, the web host will show index.html file by default; but we want it to show user the redirect.html
So we have to make changes accordingly.

#### content of *revproxy.conf* (changes are highlighted)
![](Attachments/Pasted%20image%2020230102161322.png)

Then,
![](Attachments/Pasted%20image%2020230102162717.png)
