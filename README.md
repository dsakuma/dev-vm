## Host

### Create a config.yaml
Create a config.yaml based on config.yaml.template

### Start vm
```
vagrant up
```

### SSH vm
```
vagrant ssh
```

### Configure network adapter (optional)
Use virtio-net for host-only adapter in VirtualBox for better performance

## Guest

### Add to ~/.ssh/config
```
Host *
  AddKeysToAgent yes
```
