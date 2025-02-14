## install sysdig on every node
```
curl -s https://s3.amazonaws.com/download.draios.com/stable/install-sysdig | sudo bash
```
## capturing originating from namespace 
```
sudo sysdig \
    -p "%evt.time namespace=%k8s.ns.name pod=%k8s.pod.name proc=%proc.name res=%evt.arg.res tuple=%evt.arg.tuple labels=%k8s.pod.labels" \
    "k8s.ns.name=app-routable-demo and k8s.pod.name=test and evt.type=connect"
```
## capturing originating from a pod 
```
sudo sysdig \
    -p "%evt.time namespace=%k8s.ns.name pod=%k8s.pod.name proc=%proc.name res=%evt.arg.res labels=%k8s.pod.labels port=%fd.sproto lip=%fd.lip rip=%fd.rip dport=%fd.sproto" \
    "k8s.ns.name=app-routable-demo and k8s.pod.name=test and evt.type=connect"
```
## capturing labels and TCP/UDP related info for connects (from a specific pod)
```
sudo sysdig \
    -p "%evt.time namespace=%k8s.ns.name pod=%k8s.pod.name proc=%proc.name labels=%k8s.pod.labels lip=%fd.lip rip=%fd.rip dport=%fd.sproto" \
    "k8s.ns.name=app-routable-demo and k8s.pod.name=test and evt.arg.res=0 and evt.type=connect"
```
```
14:13:11.799124013 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:11.799741149 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:11.799990390 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:11.800311070 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:11.803343805 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:11.842267704 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2606:4700:7::60 rip=:: dport=443
14:13:11.842270585 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=:: dport=443
14:13:11.842273599 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=::ffff:10.0.1.222 dport=443
14:13:11.842275664 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=:: dport=443
14:13:11.842277078 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=::ffff:10.0.1.222 dport=443
14:13:16.347388389 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:16.348005683 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:16.348229964 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:16.348461289 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:16.349435813 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=10.0.1.222 rip=10.96.0.10 dport=53
14:13:16.350293645 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2606:4700:7::60 rip=:: dport=80
14:13:16.350296458 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=:: dport=80
14:13:16.350299057 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=::ffff:10.0.1.222 dport=80
14:13:16.350301152 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=:: dport=80
14:13:16.350302478 namespace=app-routable-demo pod=test proc=curl res=0 labels=run:test lip=2a06:98c1:58::60 rip=::ffff:10.0.1.222 dport=80
```
## capturing labels and TCP/UDP related info for accepts
```
sudo sysdig \
    -p "%evt.time namespace=%k8s.ns.name pod=%k8s.pod.name proc=%proc.name labels=%k8s.pod.labels lip=%fd.lip rip=%fd.rip dport=%fd.sproto" \
    "k8s.ns.name=app-routable-demo and evt.type=accept4"
```
```
14:39:29.695478884 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
14:39:29.697738177 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
14:39:29.702989297 namespace=app-routable-demo pod=echoserver-2-deployment-5c44cf8568-7tpfs proc=node labels=app:echoserver-2, pod-template-hash:5c44cf8568 lip=10.0.1.180 rip=10.0.0.229 dport=8080
14:39:29.706947826 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
14:39:29.709677395 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
14:39:29.711391117 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
14:39:29.715444443 namespace=app-routable-demo pod=echoserver-2-deployment-5c44cf8568-7tpfs proc=node labels=app:echoserver-2, pod-template-hash:5c44cf8568 lip=10.0.1.180 rip=10.0.0.229 dport=8080
14:39:29.723628724 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
14:39:29.724820834 namespace=app-routable-demo pod=echoserver-1-deployment-6db69f7cf8-zb2jw proc=node labels=app:echoserver-1, pod-template-hash:6db69f7cf8 lip=10.0.1.203 rip=10.0.0.36 dport=8080
```

## We might corralate ... but there is the services issues !!!!
```
2291439 15:11:01.983152403 2 nginx (32557.32557) < connect res=-115(EINPROGRESS) tuple=10.0.0.140:42074->10.99.193.179:80 fd=33(<4t>10.0.0.140:42074->10.99.193.179:80)
```
```
23047918 15:11:01.983468740 2 nginx (32378.32378) < accept4 fd=19(<4t>10.0.0.140:42074->10.0.0.70:80) tuple=10.0.0.140:42074->10.0.0.70:80 queuepct=0 queuelen=0 queuemax=511
```
```
2291909 15:11:01.983275577 2 nginx (32378.32378) < connect res=-115(EINPROGRESS) tuple=10.0.0.70:35674->10.98.84.1:80 fd=11(<4t>10.0.0.70:35674->10.98.84.1:80)
```
```
23045129 15:11:01.981889593 2 nginx (32606.32606) < accept4 fd=15(<4t>10.0.0.70:35674->10.0.0.30:80) tuple=10.0.0.70:35674->10.0.0.30:80 queuepct=0 queuelen=0 queuemax=511
```

