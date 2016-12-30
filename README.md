# docker_registry

## set up

```
 $> git clone https://github.com/cdelgehier/docker_registry.git
 $> cd docker_registry
 $> vagrant up
```
## registry

 ```
 $> vagrant ssh registry
```

 Demo stuff
```
registry $>  cd /vagrant && openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key -x509 -days 365 -out certs/domain.crt
    Country Name (2 letter code) [XX]:FR
    State or Province Name (full name) []:France
    Locality Name (eg, city) [Default City]:Lille
    Organization Name (eg, company) [Default Company Ltd]:Self
    Organizational Unit Name (eg, section) []:IT
    Common Name (eg, your name or your server's hostname) []:10.0.15.10:5000         
    Email Address []:abc@vagrantmail.net
```


## node

```
node1 $>  docker pull hello-world
node1 $>  docker images
node1 $>  docker tag <IMAGE_ID> 10.0.15.10:5000/hello-earth
```
Demo stuff

```
node1 $>  sed -i '/\[ v3_ca \]/a subjectAltName = IP:10.0.15.10'  /etc/pki/tls/openssl.cnf
node1 $>  mkdir -p /etc/docker/certs.d/10.0.15.10\:5000
node1 $>  cp /vagrant/certs/domain.crt /etc/docker/certs.d/10.0.15.10\:5000/ca.crt
node1 $>  chmod -R 700 /etc/docker/certs.d/10.0.15.10\:5000

node1 $>  docker push 10.0.15.10:5000/hello-earth
```
