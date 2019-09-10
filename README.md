# Kubernetes with Vagrant machines

## Prerequisites

```bash
vagrant plugin install vagrant-vbguest
```

Simply run:
```bash
vagrant up
```

# Access the Dashboard

Run the following commands:
```bash
vagrant ssh master
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}') | egrep 'token:' | awk '{print $2}'
# copy the output
exit
```

Then visit with your browser the URL https://192.168.150.2:30000, choose Token, paste the token and then click Sign in.
