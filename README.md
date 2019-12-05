## Host

### Configure network adapter
Use virtio-net for host-only adapter in VirtualBox

### Start vm
```
vagrant up
```

### Copy keys
```
scp -i /Users/dsakuma/projects/work/safeguard/dev-vm/.vagrant/machines/default/virtualbox/private_key ~/.ssh/* vagrant@192.168.33.10:/home/vagrant/.ssh/
```
or
```
vagrant uplaod ~/.ssh/* /home/vagrant./ssh/
```

## Guest

### Add to ~/.ssh/config

```
Host *
  AddKeysToAgent yes
```

## Development options:

- Use remote-ssh plugin on vscode
- Render X on xquartz



