# dockeretcd
the docker file for etcd based on centos7


### run a standalone server, must specify the ip which client will connect to. otherwise etcdclt will not work. you also can mount the specific dir to the etcd dir 

```
docker run -d --name etcd -p 8001:4001  -e ETCD_DATA_DIR=/etcd/data -e ADV_URLS=http://192.168.3.149:8001 duffqiu/etcd
```

### run a local cluster in one container

```
docker run -d --name etcd-cluster -p 9001:9001 -p 4001:4001 -p 4002:4002 -p 4003:4003 duffqiu/etcd /etcd/start-local-cluster
```

### run mutilple nodes to setup a cluster (FIXME: how to set the advertise urls)

- run a contain with etcd daemon command and follow the cluster parameters in office document: [etcd configuration](https://github.com/coreos/etcd/blob/master/Documentation/configuration.md)
- for example, node1 `docker run -d --name etcd1 -p 4001:4001 duffqiu/etcd /etcd/etcd <parameters>`

### using etcdctl client 
- get the etcd server's ip: `etcd_ip=$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' <etcd container's name>)`
- call the client command: `docker run  --rm  duffqiu/etcd "/etcd/etcdctl -C http://$etcd_ip:4001 member list"`
- note: you have to use the quotation to include the etcdctl command and its parameters
