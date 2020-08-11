## Host

### Configure network adapter
Use virtio-net for host-only adapter in VirtualBox

### Start vm
```
vagrant up
```

### SSH vm
```
vagrant ssh
```

### Copy keys if you haven't yet

```
vagrant uplaod ~/.ssh/* /home/vagrant/.ssh/
vagrant upload ~/.aws/* /home/vagrant/.aws/
```

## Guest

### Add to ~/.ssh/config

```
Host *
  AddKeysToAgent yes
```
