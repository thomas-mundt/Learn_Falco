# Learn Falco

## Links

- https://github.com/falcosecurity/profiles
- https://falco.org/
- https://falco.org/docs/rules/supported-fields/


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


## Basic rule


falco_rules.local.yaml
```
- rule: The program "cat" is run in a container
  desc: An event will trigger every time you run cat in a container
  condition: env.type = execve and container.id != host and proc.name = cat
  output: "cat was run inside a container"
  priority: INFO
```

Test with an pod and execute cat inside the container
```
falco

#or
systemctl start falco
```



Extended rule with sysdig fields (user.name, container.name, container.image) found in falco doku or sysdig -l
```
- rule: The program "cat" is run in a container
  desc: An event will trigger every time you run cat in a container
  condition: env.type = execve and container.id != host and proc.name = cat
  output: "cat was run inside a container (user=%user.name container=%container.name image=%container.image proc=%proc.cmdline)"
  priority: INFO
```



## Use Macros

```
- macro: custom_macro
  condition: env.type = execve and container.id != host
  
- rule: The program "cat" is run in a container
  desc: An event will trigger every time you run cat in a container
  condition: custom_macro and proc.name = cat
  output: "cat was run inside a container (user=%user.name container=%container.name image=%container.image proc=%proc.cmdline)"
  priority: INFO
```


## Use Lists

```
- macro: custom_macro
  condition: env.type = execve and container.id != host
  
- lists: blacklist_binaries
  items: [cat, grep, date]
  
- rule: The program "cat" is run in a container
  desc: An event will trigger every time you run cat in a container
  condition: custom_macro and proc.name in (blacklist_binaries)
  output: "cat was run inside a container (user=%user.name container=%container.name image=%container.image proc=%proc.cmdline)"
  priority: INFO
```




## Install with Helm

```
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update

helm install falco falcosecurity/falco
```








