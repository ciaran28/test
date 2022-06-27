# test

#
# Run nginx container
#
root@docker-host-1:/home/localadmin# docker run -dit --name blue-c1 nginxdemos/hello
1c12c826586ff22e9569ca8a231e635afa60b294de7ec4b4b5bc99f6ca24520f

#
# list the container
#
root@docker-host-1:/home/localadmin# docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS         PORTS     NAMES
1c12c826586f   nginxdemos/hello   "/docker-entrypoint.…"   11 seconds ago   Up 9 seconds   80/tcp    blue-c1


#
# login to the container (Observe networking components)
#

root@docker-host-1:/home/localadmin# docker exec -it blue-c1 sh
/ # ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:10 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:876 (876.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ #
/ # route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.17.0.1      0.0.0.0         UG    0      0        0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth0

#
# Observe outbound IP of the container is the PIP of the docker hosts
#
/ # curl ifconfig.me
20.127.137.145/ # exit
#
# outbound IP on the docker-host
#
root@docker-host-1:/home/localadmin# curl ifconfig.me
20.127.137.145root@docker-host-1:/home/localadmin#
