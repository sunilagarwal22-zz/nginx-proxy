# Application Redirection

Nginx container to perform HTTP redirection to HTTPS

### Prerequisites

* Docker 
* Docker Swarm (optional)

### Technologies/Tools Used
* Docker
   - Docker being a containerization tool is robust, easy to scale, provides functionality of OS independency. To scale an application, we don’t need to create VMs but can have containers created on the fly on existing VMs.
* Docker swarm
   - Provides orchestration functionality. Takes care of all the containers across workers. 
* Existing images of 
   - nginx (alpine tag)
        Instead of re-inventing the wheel, I preferred to use nginx:alpine image as its being one of the smallest version of nginx. Does not require much time to download hence scaling up is faster.
   - http-echo 

### Installation

* **Docker Swarm** : To deploy the application, update the message (hello world) you want to return via http-echo image in web-stack.yaml file and execute below command

docker stack deploy -c web-stack.yaml <STACK_NAME>

```
Example: docker stack deploy -c web-stack.yaml myapp
```

* **Docker** : Update network name & container name in below commands and execute


docker run -dt --name <APP_NAME> --net <NETWORK> -p 5678:5678 hashicorp/http-echo -text="hello world"
docker run -dt --name <NGINX_NAME> --net <NETWORK> -p 80:80 -p 443:443 sunilthemaster/nginx:ssl

```
Example:
docker run -dt --name web1 --net mynet -p 5678:5678 hashicorp/http-echo -text="hello world $(hostname)"
docker run -dt --name webapp --net mynet -p 80:80 -p 443:443 sunilthemaster/nginx:ssl
```

## Unique features
* Error handling
* To make the site faster, have enabled http2 protocol in nginx conf

## Tests
* HTTP to HTTPS redirection: Access the URL http://<SERVER NAME>. Browser will redirect request to HTTPS. Browser will prompt for SSL certificate (as its self-signed certificate) and upon confirmation, browser will display the message provided in http-echo container
* HTTPS Access: Access the URL http://<SERVER NAME>. URL will prompt for SSL certificate (as its self-signed certificate) and upon confirmation, browser will display the message provided in http-echo container
* Remove webapp containers and then try accessing the app. Instead of showing "Bad gateway" error, application will gracefully show page not found message.


## TODO
* As of now http-echo is fixed to listen on 5678 port which can be parameterized.
* No cryptogrphic protocols specified in nginx.
