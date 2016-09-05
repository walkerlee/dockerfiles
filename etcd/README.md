## etcd

docker run --name etcd -d -p 2379:2379 -p 2380:2380 -p 4001:4001 -p 7001:7001 -v $(pwd)/data:/data walkerlee/etcd --data-dir=/data --listen-peer-urls=http://0.0.0.0:2380,http://0.0.0.0:7001 --listen-client-urls=http://0.0.0.0:2379,http://0.0.0.0:4001 --advertise-client-urls http://localhost:2379,http://localhost:4001
