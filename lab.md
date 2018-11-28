# Set-up on Laptop on Docker Hosts

* laptop
```
vboxnet0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.8.1  netmask 255.255.255.0  broadcast 172.17.8.255
```
NAT


* core-01
```
eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.8.101  netmask 255.255.0.0  broadcast 172.17.255.255
```
used for swarm communication

NAT for 2378 -> 2375 (via Vagrantfile)


* core-02
```
eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.8.102  netmask 255.255.0.0  broadcast 172.17.255.255
```
used for swarm communication

NAT for 2379 -> 2375 (via Vagrantfile)


# swarm services
## Web
```
$ docker service inspect stackdemo_web
[
    {
        "ID": "no4f64iip28pc88jef9kfpx3j",
        "Version": {
            "Index": 20
        },
        "CreatedAt": "2018-11-20T02:04:45.74483874Z",
        "UpdatedAt": "2018-11-20T02:04:45.745296788Z",
        "Spec": {
            "Name": "stackdemo_web",
            "Labels": {
                "com.docker.stack.image": "127.0.0.1:5000/stackdemo",
                "com.docker.stack.namespace": "stackdemo"
            },
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "127.0.0.1:5000/stackdemo:latest@sha256:572df2f149ad6f70b36ca57afffe07a5b2249b0d8b0df2aa5e25a358cf25bfdd",
                    "Labels": {
                        "com.docker.stack.namespace": "stackdemo"
                    },
                    "Privileges": {
                        "CredentialSpec": null,
                        "SELinuxContext": null
                    },
                    "StopGracePeriod": 10000000000,
                    "DNSConfig": {},
                    "Isolation": "default"
                },
                "Resources": {},
                "RestartPolicy": {
                    "Condition": "any",
                    "Delay": 5000000000,
                    "MaxAttempts": 0
                },
                "Placement": {
                    "Platforms": [
                        {
                            "Architecture": "amd64",
                            "OS": "linux"
                        }
                    ]
                },
                "Networks": [
                    {
                        "Target": "obkuoktfec3cvst8eh4rylmzl",
                        "Aliases": [
                            "web"
                        ]
                    }
                ],
                "ForceUpdate": 0,
                "Runtime": "container"
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 1
                }
            },
            "UpdateConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "RollbackConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "EndpointSpec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 8000,
                        "PublishedPort": 8000,
                        "PublishMode": "ingress"
                    }
                ]
            }
        },
        "Endpoint": {
            "Spec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 8000,
                        "PublishedPort": 8000,
                        "PublishMode": "ingress"
                    }
                ]
            },
            "Ports": [
                {
                    "Protocol": "tcp",
                    "TargetPort": 8000,
                    "PublishedPort": 8000,
                    "PublishMode": "ingress"
                }
            ],
            "VirtualIPs": [
                {
                    "NetworkID": "r613pls2dongh50oib5v14308",
                    "Addr": "10.255.0.4/16"
                },
                {
                    "NetworkID": "obkuoktfec3cvst8eh4rylmzl",
                    "Addr": "10.0.0.4/24"
                }
            ]
        }
    }
]
```

## Redis

```
$ docker service inspect stackdemo_redis 
[
    {
        "ID": "qj174vg8ngq9uyhfr99pyo4dm",
        "Version": {
            "Index": 28
        },
        "CreatedAt": "2018-11-20T02:04:50.796431656Z",
        "UpdatedAt": "2018-11-20T02:04:50.796942081Z",
        "Spec": {
            "Name": "stackdemo_redis",
            "Labels": {
                "com.docker.stack.image": "redis:alpine",
                "com.docker.stack.namespace": "stackdemo"
            },
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "redis:alpine@sha256:96cc76d39766ee1db844f2adc68c0122615b56bf1fa6f75a7fedcfd7ba561234",
                    "Labels": {
                        "com.docker.stack.namespace": "stackdemo"
                    },
                    "Privileges": {
                        "CredentialSpec": null,
                        "SELinuxContext": null
                    },
                    "StopGracePeriod": 10000000000,
                    "DNSConfig": {},
                    "Isolation": "default"
                },
                "Resources": {},
                "RestartPolicy": {
                    "Condition": "any",
                    "Delay": 5000000000,
                    "MaxAttempts": 0
                },
                "Placement": {
                    "Platforms": [
                        {
                            "Architecture": "amd64",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "arm64",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "386",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "ppc64le",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "s390x",
                            "OS": "linux"
                        }
                    ]
                },
                "Networks": [
                    {
                        "Target": "obkuoktfec3cvst8eh4rylmzl",
                        "Aliases": [
                            "redis"
                        ]
                    }
                ],
                "ForceUpdate": 0,
                "Runtime": "container"
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 1
                }
            },
            "UpdateConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "RollbackConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "EndpointSpec": {
                "Mode": "vip"
            }
        },
        "Endpoint": {
            "Spec": {
                "Mode": "vip"
            },
            "VirtualIPs": [
                {
                    "NetworkID": "obkuoktfec3cvst8eh4rylmzl",
                    "Addr": "10.0.0.6/24"
                }
            ]
        }
    }
]
```


# Docket networks

```
$ docker network list
NETWORK ID          NAME                DRIVER              SCOPE
fe17fa0457b4        bridge              bridge              local
bbb482778cad        docker_gwbridge     bridge              local
8007d81e40b9        host                host                local
r613pls2dong        ingress             overlay             swarm
f89fbe1fb1ae        none                null                local
37d0e6d29d6e        reg_default         bridge              local
obkuoktfec3c        stackdemo_app       overlay             swarm
```


# on containers
## Web 1
```
# docker exec stackdemo_web.1.f3x9urwe3cw8p9q2bh9yyqieg ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
20: eth0@if21: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1450 qdisc noqueue state UP 
    link/ether 02:42:0a:ff:00:05 brd ff:ff:ff:ff:ff:ff
    inet 10.255.0.5/16 brd 10.255.255.255 scope global eth0
       valid_lft forever preferred_lft forever
22: eth2@if23: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:13:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.19.0.3/16 brd 172.19.255.255 scope global eth2
       valid_lft forever preferred_lft forever
24: eth1@if25: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1424 qdisc noqueue state UP 
    link/ether 02:42:0a:00:00:05 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.5/24 brd 10.0.0.255 scope global eth1
       valid_lft forever preferred_lft forever
```

```
# docker exec stackdemo_web.1.f3x9urwe3cw8p9q2bh9yyqieg ping redis
PING redis (10.0.0.6): 56 data bytes
64 bytes from 10.0.0.6: seq=0 ttl=64 time=0.051 ms
```

## Redis 1

```
# docker exec stackdemo_redis.1.0c3rcwp0rj1e77nq1n71eigkc ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
15: eth0@if16: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1424 qdisc noqueue state UP 
    link/ether 02:42:0a:00:00:07 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.7/24 brd 10.0.0.255 scope global eth0
       valid_lft forever preferred_lft forever
17: eth1@if18: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:13:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.19.0.3/16 brd 172.19.255.255 scope global eth1
       valid_lft forever preferred_lft forever
```

```
# docker exec stackdemo_redis.1.0c3rcwp0rj1e77nq1n71eigkc ping web
PING web (10.0.0.4): 56 data bytes
```

# stackdemo_app (self defined overlay network)
## On core-01
### inspect
```
# docker network inspect stackdemo_app
[
    {
        "Name": "stackdemo_app",
        "Id": "obkuoktfec3cvst8eh4rylmzl",
        "Created": "2018-11-20T02:04:45.909415997Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "5648d58f77302e780b10d204e8e312738164d29c524d97f29ea6116d14f93616": {
                "Name": "stackdemo_web.1.f3x9urwe3cw8p9q2bh9yyqieg",
                "EndpointID": "f5f66099dbc5517ef5d58e7c6cf76bd7c894d6380b679baf3155b0001640e422",
                "MacAddress": "02:42:0a:00:00:05",
                "IPv4Address": "10.0.0.5/24",
                "IPv6Address": ""
            },
            "lb-stackdemo_app": {
                "Name": "stackdemo_app-endpoint",
                "EndpointID": "a512ad6620ebb6c0cda1d2e9248d9dc70be0ea71b4d9af102daa3c3de894ec3f",
                "MacAddress": "02:42:0a:00:00:02",
                "IPv4Address": "10.0.0.2/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097",
            "encrypted": "True"
        },
        "Labels": {
            "com.docker.stack.namespace": "stackdemo"
        },
        "Peers": [
            {
                "Name": "d93563ec9c7a",
                "IP": "172.17.8.101"
            },
            {
                "Name": "2d5c429c66ba",
                "IP": "172.17.8.102"
            }
        ]
    }
]
```
### netns

```
# ip netns exec 1-obkuoktfec ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: br0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue state UP mode DEFAULT group default 
    link/ether 72:2a:02:04:73:bf brd ff:ff:ff:ff:ff:ff
17: vxlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue master br0 state UNKNOWN mode DEFAULT group default 
    link/ether ca:5d:7e:b0:d3:1d brd ff:ff:ff:ff:ff:ff link-netnsid 0
19: veth0@if18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue master br0 state UP mode DEFAULT group default 
    link/ether aa:c8:2d:ca:01:6b brd ff:ff:ff:ff:ff:ff link-netnsid 1
25: veth1@if24: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue master br0 state UP mode DEFAULT group default 
    link/ether 72:2a:02:04:73:bf brd ff:ff:ff:ff:ff:ff link-netnsid 2
```

## On core-02
### inspect
```
# docker network inspect stackdemo_app
[
    {
        "Name": "stackdemo_app",
        "Id": "obkuoktfec3cvst8eh4rylmzl",
        "Created": "2018-11-20T02:04:50.956243965Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0d89d8bea21aeb23961a75f9cf0018e8d36786cb73236543739703c7ad5c7fb1": {
                "Name": "stackdemo_redis.1.0c3rcwp0rj1e77nq1n71eigkc",
                "EndpointID": "e27e772423110b254a0975e65038d545fba27a15714b8cef42caa70bed881a3a",
                "MacAddress": "02:42:0a:00:00:07",
                "IPv4Address": "10.0.0.7/24",
                "IPv6Address": ""
            },
            "lb-stackdemo_app": {
                "Name": "stackdemo_app-endpoint",
                "EndpointID": "fac58089ffdc684029239d291968703d3e9b56839d2f32f10ce53c0d9c75115a",
                "MacAddress": "02:42:0a:00:00:03",
                "IPv4Address": "10.0.0.3/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097",
            "encrypted": "True"
        },
        "Labels": {
            "com.docker.stack.namespace": "stackdemo"
        },
        "Peers": [
            {
                "Name": "d93563ec9c7a",
                "IP": "172.17.8.101"
            },
            {
                "Name": "2d5c429c66ba",
                "IP": "172.17.8.102"
            }
        ]
    }
]
```

### netns
```
# ip netns exec 1-obkuoktfec ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: br0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue state UP mode DEFAULT group default 
    link/ether 0e:7f:c1:a6:ed:b9 brd ff:ff:ff:ff:ff:ff
12: vxlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue master br0 state UNKNOWN mode DEFAULT group default 
    link/ether 0e:7f:c1:a6:ed:b9 brd ff:ff:ff:ff:ff:ff link-netnsid 0
14: veth0@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue master br0 state UP mode DEFAULT group default 
    link/ether f6:91:6a:83:85:a3 brd ff:ff:ff:ff:ff:ff link-netnsid 1
16: veth1@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1424 qdisc noqueue master br0 state UP mode DEFAULT group default 
    link/ether 46:d7:70:32:e7:42 brd ff:ff:ff:ff:ff:ff link-netnsid 2
```

# docker_gwbridge

Same role as docker0 in single host Docker.
Used to communicate between host and containers (Host network)

Note: two containers by host (router and ingress-sbox) with the same IP
on each host

## on core-01

```
brctl show
bridge name	bridge id		STP enabled	interfaces
br-37d0e6d29d6e		8000.02425a79af3d	no		vethbfe1e1a
docker0		8000.024286f1b0ac	no		
docker_gwbridge		8000.024269ec6e1b	no		veth10ba872
							veth663a616
```

```
# docker network inspect docker_gwbridge
[
    {
        "Name": "docker_gwbridge",
        "Id": "bbb482778cad18c253f5ecce55b9982eebc28e0574950b3d3650a136e7071412",
        "Created": "2018-11-20T01:49:16.795095269Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "5648d58f77302e780b10d204e8e312738164d29c524d97f29ea6116d14f93616": {
                "Name": "gateway_35db5cfdb362",
                "EndpointID": "7ae7c2328b5f266cbb77d7fe1d6c0062d9a632f32a28ce042497753c121eec1b",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            },
            "ingress-sbox": {
                "Name": "gateway_ingress-sbox",
                "EndpointID": "cd042fa71e98a3a73f3c8685714e59d53043d30738355b89f89c8c0cbc37a75a",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.enable_icc": "false",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.name": "docker_gwbridge"
        },
        "Labels": {}
    }
]
```

## on core-02

```
# brctl show
bridge name	bridge id		STP enabled	interfaces
docker0		8000.024206fcba68	no		
docker_gwbridge		8000.02421441b58a	no		veth50f3aa9
							vethdc2972c
```

```
# docker network inspect docker_gwbridge
[
    {
        "Name": "docker_gwbridge",
        "Id": "df1b3e2a52bcd3ef78ce63c2973de80cea9d4fc2103780a3d58951efc03e00ab",
        "Created": "2018-11-19T03:20:00.751199646Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0d89d8bea21aeb23961a75f9cf0018e8d36786cb73236543739703c7ad5c7fb1": {
                "Name": "gateway_8971f8f02c1f",
                "EndpointID": "7a5d80f654667ef3fc0aa3e008ccf7d56c2cbfe2700b5a1c786fad8d288d6670",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            },
            "ingress-sbox": {
                "Name": "gateway_ingress-sbox",
                "EndpointID": "54be80373d98cc68c7c520889de384fdb20777d0a93826d6948e20c34389c8ae",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.enable_icc": "false",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.name": "docker_gwbridge"
        },
        "Labels": {}
    }
]
```

## routing on web 1

```
# docker exec -t -i stackdemo_web.1.f3x9urwe3cw8p9q2bh9yyqieg netstat -nr
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         172.19.0.1      0.0.0.0         UG        0 0          0 eth2
10.0.0.0        0.0.0.0         255.255.255.0   U         0 0          0 eth1
10.255.0.0      0.0.0.0         255.255.0.0     U         0 0          0 eth0
172.19.0.0      0.0.0.0         255.255.0.0     U         0 0          0 eth2
```

```
# docker exec -t -i stackdemo_web.1.f3x9urwe3cw8p9q2bh9yyqieg traceroute www.google.com
traceroute to www.google.com (74.125.24.103), 30 hops max, 46 byte packets
 1  172.19.0.1 (172.19.0.1)  0.032 ms  0.006 ms  0.005 ms
 2  10.0.2.2 (10.0.2.2)  0.166 ms  0.287 ms  0.220 ms
 3  *  *  *
...
```


## routing on redis 1

```
# docker exec stackdemo_redis.1.0c3rcwp0rj1e77nq1n71eigkc netstat -nr
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         172.19.0.1      0.0.0.0         UG        0 0          0 eth1
10.0.0.0        0.0.0.0         255.255.255.0   U         0 0          0 eth0
172.19.0.0      0.0.0.0         255.255.0.0     U         0 0          0 eth1
```

```
# docker exec stackdemo_redis.1.0c3rcwp0rj1e77nq1n71eigkc traceroute www.google.com
traceroute to www.google.com (172.217.194.105), 30 hops max, 46 byte packets
 1  172.19.0.1 (172.19.0.1)  0.007 ms  0.010 ms  0.008 ms
 2  10.0.2.2 (10.0.2.2)  0.196 ms  0.711 ms  0.486 ms
...
```



# ingress

## on core-01
### inspect
```
# docker inspect ingress
[
    {
        "Name": "ingress",
        "Id": "r613pls2dongh50oib5v14308",
        "Created": "2018-11-20T01:49:16.542259759Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.255.0.0/16",
                    "Gateway": "10.255.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": true,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "5648d58f77302e780b10d204e8e312738164d29c524d97f29ea6116d14f93616": {
                "Name": "stackdemo_web.1.f3x9urwe3cw8p9q2bh9yyqieg",
                "EndpointID": "045d23f898c5d14553f9eec01e4f2a020aa43227b2206049f06906199a78f32f",
                "MacAddress": "02:42:0a:ff:00:05",
                "IPv4Address": "10.255.0.5/16",
                "IPv6Address": ""
            },
            "ingress-sbox": {
                "Name": "ingress-endpoint",
                "EndpointID": "f254e814bf9bc0047c671e7f0017b7b195bddf1b214d2b924924b2227577b277",
                "MacAddress": "02:42:0a:ff:00:02",
                "IPv4Address": "10.255.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4096"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "d93563ec9c7a",
                "IP": "172.17.8.101"
            },
            {
                "Name": "2d5c429c66ba",
                "IP": "172.17.8.102"
            }
        ]
    }
]
```

### iptables no netns

```
# iptables -t nat -nvL
Chain PREROUTING (policy ACCEPT 9 packets, 540 bytes)
 pkts bytes target     prot opt in     out     source               destination         
 3683  220K DOCKER-INGRESS  all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL
 4275  255K DOCKER     all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL

Chain INPUT (policy ACCEPT 9 packets, 540 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 5 packets, 300 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    1    60 DOCKER-INGRESS  all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL
    0     0 DOCKER     all  --  *      *       0.0.0.0/0           !127.0.0.0/8          ADDRTYPE match dst-type LOCAL

Chain POSTROUTING (policy ACCEPT 5 packets, 300 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    1    60 MASQUERADE  all  --  *      docker_gwbridge  0.0.0.0/0            0.0.0.0/0            ADDRTYPE match src-type LOCAL
    0     0 MASQUERADE  all  --  *      !br-37d0e6d29d6e  172.20.0.0/16        0.0.0.0/0           
   83  4661 MASQUERADE  all  --  *      !docker_gwbridge  172.19.0.0/16        0.0.0.0/0           
    6   356 MASQUERADE  all  --  *      !docker0  172.18.0.0/16        0.0.0.0/0           
    0     0 MASQUERADE  tcp  --  *      *       172.20.0.2           172.20.0.2           tcp dpt:5000

Chain DOCKER (2 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 RETURN     all  --  br-37d0e6d29d6e *       0.0.0.0/0            0.0.0.0/0           
    0     0 RETURN     all  --  docker_gwbridge *       0.0.0.0/0            0.0.0.0/0           
    0     0 RETURN     all  --  docker0 *       0.0.0.0/0            0.0.0.0/0           
    0     0 DNAT       tcp  --  !br-37d0e6d29d6e *       0.0.0.0/0            0.0.0.0/0            tcp dpt:5000 to:172.20.0.2:5000

Chain DOCKER-INGRESS (2 references)
 pkts bytes target     prot opt in     out     source               destination         
    1    60 DNAT       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8000 to:172.19.0.2:8000
 3683  220K RETURN     all  --  *      *       0.0.0.0/0            0.0.0.0/0           

Chain vl (0 references)
 pkts bytes target     prot opt in     out     source               destination         
```

in DOCKER-INGRESS: DNAT *:8000 to 172.19.0.2:8000 (ingress-sbox addr on docker_gwbridge)

## on core-02
### inspect
```
# docker inspect ingress
[
    {
        "Name": "ingress",
        "Id": "r613pls2dongh50oib5v14308",
        "Created": "2018-11-20T01:50:16.780581497Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.255.0.0/16",
                    "Gateway": "10.255.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": true,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "ingress-sbox": {
                "Name": "ingress-endpoint",
                "EndpointID": "e43d0f2a1d82fb909e12ac5e3620814bc4799935f77f64a421776baf53d1b995",
                "MacAddress": "02:42:0a:ff:00:03",
                "IPv4Address": "10.255.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4096"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "d93563ec9c7a",
                "IP": "172.17.8.101"
            },
            {
                "Name": "2d5c429c66ba",
                "IP": "172.17.8.102"
            }
        ]
    }
]
```

### iptables no netns
```
# iptables -t nat -nvL
Chain PREROUTING (policy ACCEPT 4125 packets, 247K bytes)
 pkts bytes target     prot opt in     out     source               destination         
 4008  241K DOCKER-INGRESS  all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL
 4449  267K DOCKER     all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL

Chain INPUT (policy ACCEPT 4008 packets, 241K bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 911 packets, 55123 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    2   144 DOCKER-INGRESS  all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL
    2   144 DOCKER     all  --  *      *       0.0.0.0/0           !127.0.0.0/8          ADDRTYPE match dst-type LOCAL

Chain POSTROUTING (policy ACCEPT 910 packets, 55039 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    1    60 MASQUERADE  all  --  *      docker_gwbridge  0.0.0.0/0            0.0.0.0/0            ADDRTYPE match src-type LOCAL
    0     0 MASQUERADE  all  --  *      !docker0  172.18.0.0/16        0.0.0.0/0           
   97  5107 MASQUERADE  all  --  *      !docker_gwbridge  172.19.0.0/16        0.0.0.0/0           

Chain DOCKER (2 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 RETURN     all  --  docker0 *       0.0.0.0/0            0.0.0.0/0           
    0     0 RETURN     all  --  docker_gwbridge *       0.0.0.0/0            0.0.0.0/0           

Chain DOCKER-INGRESS (2 references)
 pkts bytes target     prot opt in     out     source               destination         
    1    60 DNAT       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8000 to:172.19.0.2:8000
 4009  241K RETURN     all  --  *      *       0.0.0.0/0            0.0.0.0/0
 ```

# ingress s-box netns

## on core-01

```
# ip netns exec ingress_sbox ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
7: eth0@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default 
    link/ether 02:42:0a:ff:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.255.0.2/16 brd 10.255.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet 10.255.0.4/32 brd 10.255.0.4 scope global eth0
       valid_lft forever preferred_lft forever
10: eth1@if11: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:13:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet 172.19.0.2/16 brd 172.19.255.255 scope global eth1
       valid_lft forever preferred_lft forever
```

```
# ip netns exec ingress_sbox iptables -nvL -t mangle
Chain PREROUTING (policy ACCEPT 10 packets, 780 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    5   346 MARK       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8000 MARK set 0x100

Chain INPUT (policy ACCEPT 5 packets, 346 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 MARK       all  --  *      *       0.0.0.0/0            10.255.0.4           MARK set 0x100

Chain FORWARD (policy ACCEPT 5 packets, 434 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 5 packets, 346 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain POSTROUTING (policy ACCEPT 10 packets, 780 bytes)
 pkts bytes target     prot opt in     out     source               destination         
```

Update: nat table was missing

```
# ip netns exec ingress_sbox ipvsadm -ln            
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
FWM  256 rr
  -> 10.255.0.5:0                 Masq    1      0          0
 ``` 


### on core-02

```
# ip netns exec ingress_sbox ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
8: eth0@if9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default 
    link/ether 02:42:0a:ff:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.255.0.3/16 brd 10.255.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet 10.255.0.4/32 brd 10.255.0.4 scope global eth0
       valid_lft forever preferred_lft forever
10: eth1@if11: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:13:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet 172.19.0.2/16 brd 172.19.255.255 scope global eth1
       valid_lft forever preferred_lft forever
```

```
# ip netns exec ingress_sbox iptables -nvL -t mangle
Chain PREROUTING (policy ACCEPT 12 packets, 1045 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    6   410 MARK       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8000 MARK set 0x100

Chain INPUT (policy ACCEPT 6 packets, 410 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 MARK       all  --  *      *       0.0.0.0/0            10.255.0.4           MARK set 0x100

Chain FORWARD (policy ACCEPT 6 packets, 635 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 6 packets, 410 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain POSTROUTING (policy ACCEPT 12 packets, 1045 bytes)
 pkts bytes target     prot opt in     out     source               destination
 ```

 update: nat table was missing

```
# ip netns exec ingress_sbox iptables -t nat -nvL
Chain PREROUTING (policy ACCEPT 42 packets, 2520 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER_OUTPUT  all  --  *      *       0.0.0.0/0            127.0.0.11          

Chain POSTROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER_POSTROUTING  all  --  *      *       0.0.0.0/0            127.0.0.11          
   45  2700 SNAT       all  --  *      *       0.0.0.0/0            10.255.0.0/16        ipvs to:10.255.0.3

Chain DOCKER_OUTPUT (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DNAT       tcp  --  *      *       0.0.0.0/0            127.0.0.11           tcp dpt:53 to:127.0.0.11:36391
    0     0 DNAT       udp  --  *      *       0.0.0.0/0            127.0.0.11           udp dpt:53 to:127.0.0.11:48122

Chain DOCKER_POSTROUTING (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 SNAT       tcp  --  *      *       127.0.0.11           0.0.0.0/0            tcp spt:36391 to::53
    0     0 SNAT       udp  --  *      *       127.0.0.11           0.0.0.0/0            udp spt:48122 to::53

```
# ip netns exec ingress_sbox ipvsadm -ln            
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
FWM  256 rr
  -> 10.255.0.5:0                 Masq    1      0          0         
```


# SNAT troubles

Something rewrites source address when we browse application server form laptop.

Source address is either 10.225.0.2 (when browsing 172.17.8.101)
or 10.255.03 (when browsing 172.17.8.102)

## investigation on core-02

### tcpdump eth1

```
# tcpdump -n -i eth1 tcp port 8000
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), capture size 262144 bytes
08:20:56.680588 IP 172.17.8.1.60968 > 172.17.8.102.8000: Flags [S], seq 1261821964, win 29200, options [mss 1460,sackOK,TS val 1401319362 ecr 0,nop,wscale 7], length 0
```

OK

### tcpdump docker_gwbridge0

```
# tcpdump -n -i docker_gwbridge tcp port 8000
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on docker_gwbridge, link-type EN10MB (Ethernet), capture size 262144 bytes
08:25:14.279614 IP 172.17.8.1.60984 > 172.19.0.2.8000: Flags [S], seq 2087511167, win 29200, options [mss 1460,sackOK,TS val 1401576969 ecr 0,nop,wscale 7], length 0
```

DNAT as seen: OK

### tcpdump netns ingress_sbox eth1

```
# ip netns exec ingress_sbox tcpdump -i eth1 -n tcp port 8000
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), capture size 262144 bytes
08:26:40.197677 IP 172.17.8.1.32770 > 172.19.0.2.8000: Flags [S], seq 165477545, win 29200, options [mss 1460,sackOK,TS val 1401662889 ecr 0,nop,wscale 7], length 0
```

OK

### tcpdump netns ingress_sbox eth0

```
# ip netns exec ingress_sbox tcpdump -i eth0 -n tcp port 8000
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
08:27:24.322876 IP 10.255.0.3.32776 > 10.255.0.9.8000: Flags [S], seq 1634548471, win 29200, options [mss 1460,sackOK,TS val 1401707016 ecr 0,nop,wscale 7], length 0
```

KO SADDR changed

Note: we have booted many times since the beginning, so the Real IPs are:

```
# ip netns exec ingress_sbox ipvsadm -ln 
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
FWM  261 rr
  -> 10.255.0.9:0                 Masq    1      0          2         
FWM  264 rr
  -> 10.255.0.10:0                Masq    1      0          0
```


It seems that the trouble is here:

```
# ip netns exec ingress_sbox iptables -t nat -nvL
...

Chain POSTROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER_POSTROUTING  all  --  *      *       0.0.0.0/0            127.0.0.11          
   45  2700 SNAT       all  --  *      *       0.0.0.0/0            10.255.0.0/16        ipvs to:10.255.0.3

```

It is not a bug, it is need for routing mesh, wish use S/NAT LVS

http://www.loadbalancer.org/blog/enabling-snat-in-lvs-xt_ipvs-and-iptables/
http://www.loadbalancer.org/blog/load-balancing-methods/

It seems that the only advantage of this is zero configuration needed on real servers

Without this, a packet would be in trouble for coming back, example:

Client hits node3:app_port, the routing mesh send this app_server who runs only one node2.
When the packet goes back, it will have node2 IP as SRC ADDR.
