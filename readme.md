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
## capturing labels and TCP/UDP related info
```
sudo sysdig \
>     -p "%evt.time namespace=%k8s.ns.name pod=%k8s.pod.name proc=%proc.name labels=%k8s.pod.labels lip=%fd.lip rip=%fd.rip dport=%fd.sproto" \
>     "k8s.ns.name=app-routable-demo and k8s.pod.name=test and evt.arg.res=0 and evt.type=connect"
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
