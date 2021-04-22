# Learn Falco


## Install Falco

```
curl -s https://falco.org/repo/falcosecurity-3672BA8F.asc | apt-key add -
echo "deb https://download.falco.org/packages/deb stable main" | tee -a /etc/apt/sources.list.d/falcosecurity.list
apt-get update -y
apt-get -y install linux-headers-$(uname -r)
apt-get -y install falco
```

```
systemctl start falco
systemctl enable falco

# Or in for debugging
falco
```



## How to use falco

In one tab (look at this terminal while you execute commands in second tab)
```
falco
```


In a second tab
```
cat /etc/shadow

kubectl run nginx --image nginx
Kubectl exec -it nginx -- sh
```


## Edit rules

```
cd /etc/falco
vi falco_rules.yaml
```

Link:
https://github.com/falcosecurity/profiles
