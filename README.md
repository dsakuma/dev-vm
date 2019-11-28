## Host

### Configure network adapter
Use virtio-net for host-only adapter in VirtualBox

### Start vm
```
$ vagrant up
```


### Copy keys
```
$ scp -i /Users/dsakuma/projects/work/safeguard/dev-vm/.vagrant/machines/default/virtualbox/private_key ~/.ssh/* vagrant@192.168.33.10:/home/vagrant/.ssh/
```
or
```
$ vagrant uplaod ~/.ssh/* /home/vagrant./ssh/
```

## Guest

### Install docker
```
curl -fsSL https://get.docker.com | s
```

```
sudo usermod -aG docker vagrant
```

### Install docker-compose
```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
````

### Start nginx-proxy
```
$ docker run -d -t -p 80:80 --restart=always --volume /var/run/docker.sock:/tmp/docker.sock:ro --name=nginx-domain-proxy jwilder/nginx-proxy
```

## Development options:

- Use remote-ssh plugin on vscode
- Render X on xquartz



