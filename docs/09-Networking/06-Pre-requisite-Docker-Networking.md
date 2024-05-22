# Pre-requisite Docker Networking

  - Take me to [Lecture](https://kodekloud.com/topic/prerequsite-docker-networking/)
Instructor: Hello, and welcome to this lecture.

In this lecture, we look at networking in Docker.

We will start with the basic networking options in Docker

and then try and relate it

to the concepts around networking namespaces.

Let's start with a single Docker host,

a server with Docker installed on it.

It has an internet interface at 80 that connects

to the local network

with the IP address 192.168.1.10.

When you run a container,

you have different networking options to choose from.

First, let's see the none network.

With a none network, the Docker container

is not attached to any network.

The container cannot reach the outside world,

and no one from the outside world can reach the container.

If you're on multiple containers,

they are all created without being part of any network

and cannot talk to each other or to the outside world.

Next is the host network.

With the host network, the container is attached

to the host network.

There is no network isolation

between the host and the container.

If you deploy a web application listening

on port 80 in the container,

then the web application is available on port 80 on the host

without having to do any additional port mapping.

If you try to run another instance of the same container

that listens on the same port,

it won't work as they share the host networking,

and two processes cannot listen

on the same port at the same time.

The third networking option is the bridge.

In this case, an internal private network is created

which the Docker host and containers attach to.

The network has an address 172.17.0.0 by default,

and each device connecting to this network get

their own internal private network address on this network.

This is the network that we are most interested in,

so we will take a deeper look at how exactly

Docker creates and manages this network.

When Docker is installed on the host,

it creates an internal

private network called Bridge by default.

You can see this

when you're the Docker network ls command.

Now, Docker calls the network by the name Bridge,

but on the host, the network is created

by the name Docker0.

You can see this in the output of the IP link command.

Docker internally uses a technique similar to what we saw

in the video on namespaces by running the IP link

add command with the type set to bridge.

So, remember, the name bridge

in the Docker network ls output

refers to the name Docker0 on the host.

They are one and the same thing.

Also note that the interface, or network, is currently down.

Now, remember, we said that the bridge network is

like an interface to the host but a switch

to the namespaces or containers within the host.

So the interface Docker0 on the host

is assigned an IP 172.17.0.1.

You can see this in the output of the IP ADDR command.

Whenever a container is created,

Docker creates a network namespace for it.

Just like how we created network namespaces

in the previous video.

Run the IP netns command to list the namespace.

Note that there is a minor hack to be done

to get the IP netns command to list

the namespaces created by Docker.

Check out the resources section of this lecture

for information on that.

The namespace has the name starting B3165.

You can see the namespace associated with each container

in the output of the Docker inspect command.

So how does Docker attach the container,

or its network namespace, to the bridge network?

For the remainder of this lecture,

a container and network namespace means the same thing.

When I say container, I'm referring

to the network namespace created

by Docker for that container.

So how does Docker attach the container to the bridge?

As we did before, it creates a cable, a virtual cable,

with two interfaces on each end.

Let's find out what Docker has created here.

If you run the IP link command on the Docker host,

you see one end of the interface

which is attached to the local bridge, Docker0.

If you run the same command again,

this time with the dash end option with a namespace,

then it lists the other end

of the interface within the container namespace.

The interface also gets an IP assigned within the network.

You can view this by running the IP ADDR command,

but within the container's namespace.

The container gets assigned 172.17.0.3.

You can also view this by attaching to the container

and looking at the IP address assigned to it that way.

The same procedure is followed

every time a new container is created.

Docker creates a namespace, creates a pair of interfaces,

attaches one end to the container

and another end to the bridge network.

The interface pairs can be identified using their numbers.

Odd and even form a pair.

Nine and ten are one pair.

Seven and eight are another.

Eleven and 12 are one pair.

The containers are all part of the network now.

They can all communicate with each other.

Let us look at port mapping now.

The container we created is Nginx,

so it's a web application serving webpage on port 80.

Since our container is within a private network

inside the host, only other containers in the same network,

or the host itself, can access this webpage.

If you try to access the webpage using curl with the IP

of the container from within Docker host on port 80,

you will see the webpage.

If you try to do the same thing outside the host,

you cannot view the webpage.

To allow external users to access the applications

hosted on containers, Docker provides a port publishing,

or port mapping, option.

When you run containers, tell Docker to map

port 8080 on the Docker host to port 80 on the container.

With that done, you could access the web application

using the IP of the Docker host and port 8080.

Any traffic to port 8080 on the Docker host

will be forwarded to port 80 on the container.

Now all of your external users

and other applications or servers can use this URL

to access the application deployed on the host.

But how does Docker do that?

How does it forward traffic from one port to another?

Well, what would you do?

Let's forget about Docker and everything else for a second.

The requirement is to forward traffic coming

in on one port to another port on the server.

We talked about it in one of our prerequisite lectures.

We create a NAT rule for that.

Using IP tables, we create an entry

into the NATs table to append the rules

to the prerouting chain to change

the destination port from 8080 to 80.

Docker does it the same way.

Docker adds the rule to the Docker chain

and sets destination to include the container's IP as well.

You can see the rule Docker creates

when you list the rules in IP tables.

Well, that's it for this lecture.

In the next lecture, we will talk

about CNI and what a Container Networking Interface is.


============================================================================================



In this section, we will take a look at **Docker Networking**

## None Network

- Running docker container with `none` network

```
$ docker run --network none nginx
```

## Host Network

- Running docker container with `host` network

```
$ docker run --network host nginx
```

## Bridge Network

- Running docker container with `bridge` network

```
$ docker run --network bridge nginx
```

## List the Docker Network

```
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
4974cba36c8e        bridge              bridge              local
0e7b30a6c996        host                host                local
a4b19b17d2c5        none                null                local

```

## To view the Network Device on the Host  

```
$ ip link
or
$ ip link show docker0
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default
    link/ether 02:42:cf:c3:df:f5 brd ff:ff:ff:ff:ff:ff
```

- With the help of `ip link add` command to type set `bridge` to `docker0`

```
$ ip link add docker0 type bridge
```

## To view the IP Addr of the interface docker0

```
$ ip addr
or
$ ip addr show docker0
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:cf:c3:df:f5 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/24 brd 172.18.0.255 scope global docker0
       valid_lft forever preferred_lft forever
```

## Run the command to create a Docker Container

```
$ docker run nginx
```

## To list the Network Namespace

```
$ ip netns
1c452d473e2a (id: 2)
db732004aa9b (id: 1)
04acb487a641 (id: 0)
default

# Inspect the Docker Container

$ docker inspect <container-id>

# To view the interface attached with the local bridge docker0

$ ip link
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default
    link/ether 02:42:c8:3a:ea:67 brd ff:ff:ff:ff:ff:ff
5: vetha3e33331@if3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP mode DEFAULT group default
    link/ether e2:b2:ad:c9:8b:98 brd ff:ff:ff:ff:ff:ff link-netnsid 0

# with -n options with the network namespace to view the other end of the interface

$ ip -n 04acb487a641 link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
3: eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default
    link/ether c6:f3:ca:12:5e:74 brd ff:ff:ff:ff:ff:ff link-netnsid 0

# To view the IP Addr assigned to this interface 

$ ip -n 04acb487a641 addr
3: eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether c6:f3:ca:12:5e:74 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.244.0.2/24 scope global eth0
       valid_lft forever preferred_lft forever
```

## Port Mapping

- Creating a docker container.

```
$ docker run -itd --name nginx nginx
d74ca9d57c1d8983db2c590df2fdd109e07e1972d6b361a6ecad8a942af5bf7e
```

- Inspect the docker container to view the IPAddress.

```
$ docker inspect nginx | grep -w IPAddress
            "IPAddress": "172.18.0.6",
                    "IPAddress": "172.18.0.6",
```

- Accessing web page with the `curl` command.

```
$ curl --head  http://172.18.0.6:80
HTTP/1.1 200 OK
Server: nginx/1.19.2
```

- Port Mapping to docker container

```
$ docker run -itd --name nginx -p 8080:80 nginx
e7387bbb2e2b6cc1d2096a080445a6b83f2faeb30be74c41741fe7891402f6b6
```

- Inspecting docker container to view the assgined ports.

```
$ docker inspect nginx | grep -w -A5 Ports

  "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "8080"
                    }

```
- To view the IP Addr of the host system

```
$ ip a

# Accessing nginx page with curl command

$ curl --head http://192.168.10.11:8080
HTTP/1.1 200 OK
Server: nginx/1.19.2
```

- Configuring **iptables nat** rules

```
$ iptables \
         -t nat \
         -A PREROUTING \
         -j DNAT \
         --dport 8080 \
         --to-destination 80
```

```
$ iptables \
      -t nat \
      -A DOCKER \
      -j DNAT \
      --dport 8080 \
      --to-destination 172.18.0.6:80
```

## List the Iptables rules

```
$ iptables -nvL -t nat
```




#### References docs

- https://docs.docker.com/network/
- https://linux.die.net/man/8/iptables
- https://linux.die.net/man/8/ip
