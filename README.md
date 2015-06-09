# docker-nginx

1. Create a new docker file


    FROM martin/nginx

2. Place nginx site config file in directory `./conf`, these will be placed in `/etc/nginx/conf.d/`. the config file should have server root set to /app/ `root /app/;`
3. Place html files in directory "./app", this will be your main site

4. `docker build -t mynew/nginx .`
5. `docker run -d mynew/nginx`

Config files may contain environment variables in the form of `$ENV{"environmentvariablename"}`. These will be replaced when the container starts.
So if you link another container `docker run -d --link myservicecontainer mynew/nginx`
    
    server {
            listen 80;
            root /app/;
            index index.html;
            
            location /myservice {
                    proxy_pass      http://myservice_backend;
            }
          
    }

    upstream myservice_backend {
            server myservicecontainer:$ENV{"MYSERVICECONTAINER_PORT_1234_TCP_PORT"};
            keepalive 4;
    }
