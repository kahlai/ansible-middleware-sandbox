How to build

docker build -t quay.io/kahlai/ssh-docker:centos8 .

Testing as standalone 
docker run --rm --privileged -v /sys/fs/cgroup:/sys/fs/cgroup:ro -p 2222:22 quay.io/kahlai/ssh-docker:centos8

ssh root@localhost -p 2222