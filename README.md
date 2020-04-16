## Host

### Configure network adapter
Use virtio-net for host-only adapter in VirtualBox

### Start vm
```
vagrant up
```

### Copy keys

From dev-vm project folder:

```
scp -r -i .vagrant/machines/default/virtualbox/private_key ~/.aws vagrant@192.168.33.10:/home/vagrant/.aws
scp -r -i .vagrant/machines/default/virtualbox/private_key ~/.aws vagrant@192.168.33.10:/home/vagrant/.aws
```
or
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

## Development options:

- Use remote-ssh plugin on vscode
- Render X on xquartz



