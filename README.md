# Debian/stretch64 Vragrant box With French GUI


## What ?

This repo contains the necessary provisioning to create a french configured debian box using stretch64 debian official box.

This configures:

- The keyboard
- The locale
- KDE
- A User on another volume

## VirtualBox Guest Additions

```
$ vagrant plugin install vagrant-vbguest
```

## How to Run ?

```bash
$ vagrant up

# If GUI doesnt start, login and run startx
$ startx
```

## Default access

User: vagrant  
Pass: vagrant
