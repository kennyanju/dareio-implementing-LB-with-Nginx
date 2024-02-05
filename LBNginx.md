# Implementing a Load Balancer with Nginx

## Set up the webservers

Launch 2 EC2 instances to host the webservers (webserver1 and webserver2) and 1 instance to host the load balancer.

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/84147632-63df-4eb2-9b1b-5aa6b95cb5a8)


```
// Update and install Apache on the webservers
sudo apt update -y && sudo apt install apache2 -y  
```

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/cd6f470e-dcd6-48d1-8134-9c7933cc0211)


```
// Check if Apache installed correctly 
sudo systemctl status apache2
```

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/272eaef2-3032-46b0-a0c8-04b4dcbe8e4b)


Configure Apache to listen on port 8000 and the default 80:

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/539ce438-703b-4c7b-a272-017c42a625b0)


```
// Update Apache config file with new port  
sudo vi /etc/apache2/ports.conf
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/9ba8c2d1-6e14-4c74-abb4-26e89cde4256)



```
// Update default config file with new port
sudo vi /etc/apache2/sites-available/000-default.conf
```

Create a simple web page and set permissions:

``` 
// Create a demo index.html page
sudo vi index.html
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/8ae7eaf6-0d15-4147-880a-96fb94088363)


```
// Set correct permissions  
sudo chown www-data:www-data ./index.html
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/28992a8c-8d18-4a36-8c4d-01bc0cdc16b4)

Restart Apache after config changes:

```
sudo systemctl restart apache2
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/692bb7fe-0118-4e94-8007-dfc04f997691)

At this point you should be able to access the webservers from the browser using the server IP addresses.

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/b2ce3b18-7b80-4dd5-8099-cc7116bb6761)

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/ea7e6a13-bba3-44f6-a0b5-f5ab162d4e40)



## Set up the Load Balancer

On the load balancer instance, install and configure Nginx: 

```
// Install Nginx
sudo apt update -y && sudo apt install nginx -y
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/2eca66a6-e3e6-4f2e-8e9a-8dc680546d66)


```
// Check Nginx status 
sudo systemctl status nginx
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/bf76d4c2-fe2b-4235-847d-a6d92abd4e80)

Update the Nginx config to include the IP addresses of the 2 webservers:

![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/0ef438f3-4db7-4039-9af2-ad3643bf245f)


```
// Add webserver IPs 
sudo vi /etc/nginx/conf.d/loadbalancer.conf  
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/fb2db815-0bbe-416a-a48c-70059b95652b)


Check config and restart Nginx:

```
// Validate config
sudo nginx -t   
```

```
// Restart 
sudo systemctl restart nginx
```
![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/96356d32-616a-41a9-ae21-bdc4ad39079b)

Now, when you access the load balancer IP, you should see it switch between webserver1 and webserver2.


![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/811fd5d4-24fb-49a7-bd9f-c067762a39cb)


![image](https://github.com/kennyanju/dareio-implementing-LB-with-Nginx/assets/10983149/e0c774be-545d-4355-9d05-30c4ed5c5b93)


Let me know if any part needs more explanation!
