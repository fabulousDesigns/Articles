## Learn How to Set up Linux's Quagga Routing Suite

Quagga is a popular open-source project for providing routing services on the Linux platform. It is made up of several components for various dynamic protocols.
Such as Open Shortest Path First (OSPF), Routing Information Protocol (RIP), Border Gateway Protocol (BGP), Intermediate System to Intermediate System (IS-IS), and Multiprotocol Label Switching (MPLS). It partially uses the same virtual terminal or CLI (vty/vtysh) for protocol configuration as Cisco/Juniper.

in this article, we will install the Quagga routing suite on Ubuntu.

### Objectives
By the end of this article, you should be able to:
- Know what is Quagga Linux and its brief history.
- Understand the working of Quagga Linux.
- Know why you should use Quagga Linux.
- Supported Platforms by Quagga Linux.
- Install Quagga in a Linux environment.
- Configure the Zebra daemon, Ospfd daemon, and Ripd daemon.

### A quick history of Quagga and an explanation of what it is
Quagga is a GPLv2-licensed open-source routing stack. It is a program that implements IP routing protocols like `RIP`, `RIPng`, `OSPF`, and `ISIS`.

Let's look at the distinction between a `routing stack` and a `complete router.` A routing stack is a set of protocols that transmit routing information between network nodes so that each node has the necessary knowledge of the network architecture, whereas a complete router is a networking device that routes data packets between computer networks.

A complete router in Quagga requires traffic forwarding and a routing stack, however, only routing protocols are implemented.

Quagga can run on Linux and forward traffic using the standard Linux kernel, or it can use OpenFlow or another open proprietary interface to connect to a distributed forwarding platform. It can also be used with off-the-shelf routers to accept and advertise routes for routing protocols.

Quagga evolved from the `Zebra` routing code about 10 years ago. `Zebra` is no longer a public project, but it is still available as a commercial solution under the name `ZebOS` from `IP Infusion.`

### How does Quagga work?
A system with Quagga installed acts as a dedicated router. With Quagga, your machine exchanges routing information with other routers using routing protocols.

Quagga uses this information to update the kernel routing table so that the right data goes to the intended place.

### Who uses Quagga today and what other open-source alternatives are available?
Quagga is primarily used in virtual environments, large data centers (cloud providers), and the academic/research community. It is used where an open-source implementation of routing protocols is required as a foundation for experimenting with new standards and ideas.

Regrettably, the majority of large users use Quagga in secret and they're not going to admit it in public. They use Quagga because it allows them to tailor it to their network's specific needs.

`Bird` is the most viable open-source alternative to Quagga as far as viable open-source alternatives go. `Bird` began as a `BGP route-server/reflector` for Internet service providers and exchange points and it has a lot of features that are unique to this environment.

`Bird` has evolved into a more generic routing stack in recent years and it has long supported `OSPF`, and `ISIS` is in the works. `Bird` is most commonly used in environments where the system only receives and sends `BGP`.

Aside from `Bird`, there's `XORP`, which has a small but active community, as well as the OpenBSD-supported `OpenBGPd` and `OpenOSPFd` projects.

### Supported Platforms
Currently, Quagga supports `GNU/LINUX` and `BSD`. Porting Quagga to other platforms is too difficult as platform-dependent code is limited to the `zebra` daemon.

Protocol daemons are mostly platform-independent. The list of officially supported platforms are listed below:
- GNU/Linux
- FreeBSD
- NetBSD
- OpenBSD

> Quagga may run correctly on other platforms and may run with partial functionality on further platforms. Versions of these platforms that are older than around 2 years from the point of their original release may need some work.

Similarly, the following platforms may work with some effort:
- Solaris
- Mac OSX

> Also note that particularly regarding proprietary platforms, compiler and `C` library choice will affect Quagga. Only recent versions of the following `C` compilers are well tested:
- GNU'S GCC
- LLVM'S clang
- intel's ICC

### Installation
Quagga can be installed from the source code, but we will use the `deb/binary` package in this article. To check the Qugaaga package's dependencies, run the command below:

```bash
apt-cache depends quagga
```

- The output of the above command is shown below:


![installation.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404495235/TmeibM2j-.png align="left")

- Run the following command to install Qugaaga routing software:

```bash
apt-get install quagga
```

Quagga package installation is shown in the following figure:


![installation2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404521097/UP7GbRCnD.png align="left")

### Configuration
Unless a few kernel settings are enabled, Linux-based devices do not support packet forwarding by default. Using the commands indicated in the figure below, enable packet forwarding for IPv4. Settings will be permanently saved in `/etc/sysctl.conf` file.

```bash
echo "net.ipv4.conf.all.forwarding=1" | sudo tee -a /etc/sysctl.conf
echo "net.ipv4.conf.default.forwarding=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```


![configuration.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404560977/izyEZPjS7.png align="left")

We will now configure Quagga routing software to run on Linux after enabling packet forwarding. To run the Quagga daemon on Ubuntu, you'll need to do the following:

```bash
ripngd.conf
ospfd.conf
bgpd.conf
ripd.conf
zebra.conf
vytsh.conf
isis.conf
babelid.conf
```

A user can create these configuration files or copy these sample files from `/usr/share/doc/quagga/examples/` path to `/etc/quagga/*.conf` path.


![config2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404585576/5fY0IUVey.png align="left")

Copying sample files under `/etc/quagga/` path is shown in the following figure:


![duplicate-records.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404608407/w1pBofbY7.png align="left")

Rename sample files after copying in `/etc/quagga` directory.

```bash
sudo mv ripngd.conf.sample ripngd
sudo mv ospfd.conf.sample ospfd.conf
sudo mv bgpd.conf.sample bgpd.conf
sudo mv ripd.conf.sample ripd.conf
sudo mv zebra.conf.sample zebra.conf
sudo mv vtysh.conf.sample vtysh.conf
sudo mv isisd.conf.sample isisd.conf
sudo mv babeld.conf.sample babeld.conf
```


![rename.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404641153/5TC-wASTC.png align="left")

Because the Quagga daemon runs as a quagga, it changes the ownership and permissions of configuration files. During package installation, Ubuntu creates the user quagga.


![daemons.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404685003/OXaPxfCI6.png align="left")

Run the following command under the `/etc/quagga` folder to change ownership and permission:

```bash
sudo chown quagga:quagga *
```

![user-group.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404713813/Wxs9jYO8m.png align="left")

The permission "640" has already been assigned to the files. Finally, we must enable or disable Quagga's many daemons. This is because the Zebra daemon is an essential component of the routing suite, it must be enabled in the /etc/quagga/daemons file.

Only OSPF and RIP dynamic protocol daemons are enabled in the file in this post.

The following snapshot depicts the default setting of the daemons file. The default configuration file disables all routing daemons.


![daemons-file.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404737564/KvNWX4Gom.png align="left")

The Configuration file with OSPF and RIP enabled is shown below:


![df2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404776976/MDGxIq8Zj.png align="left")

Different daemons of the Quagga suite will run on the TCP protocol and listening ports will be from 2600-2800.

```bash
cat /etc/services | grep zebra

zebrasrv       2600/tcp      # zebra service
zebra          2601/tcp      # zebra vty
ripd           2602/tcp      # ripd vty (zebra)
ripngd         2603/tcp      # ripngd vty (zebra)
ospfd          2604/tcp      # ospfd vty (zebra)
bgpd           2605/tcp      # bgpd vty (zebra)
ospf6d         2606/tcp      # ospf6d vty (zebra)
isisd          2608/tcp      # ISISd vty (zebra)
```


![services.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404807519/xT2x1nln4.png align="left")

Start Quagga routing suite using the following command:

```bash
sudo /etc/init.d/quagga restart
```


![quagga-starting.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404835558/Nwa26wW4Z.png align="left")

Using the netstat command, we can confirm the successful running of daemons.

```bash
sudo netstat -antp | grep 260
```


![netstat.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404853375/7rrQRBZCA.png align="left")

Quagga routing can be configured using the following ways:

```bash
vtysh

telnet (telnet 127.0.0.1 ospfd/ripd/bgpd/zebra)
```

vtysh provides a central location for all daemon configuration. To start a virtual shell (vtysh) for Quagga configuration, type the following command in a terminal:

```bash
sudo vtysh
```


![vtysh.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404897618/YOhPAqCKR.png align="left")

Zebra, ospfd and ripd can be configured using vtysh.


![available-daemons-in-vtysh.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404921887/MqnopT0AW.png align="left")

Now, let us look at daemon configurations using telnet commands.

#### Zebra daemon
The Quagga architecture is made up of a core daemon called `Zebra`. Zebra functions as an abstraction layer for the underlying Unix kernel and exposes the `Zserv` API to Quagga clients through a `Unix` or `TCP` stream. These Zserv clients are usually responsible for implementing a routing protocol and communicating routing updates to the zebra daemon.

The command below shows how to configure `zebra` accordingly.

```bash
telnet localhost 2601
```


![zebra-telnet.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660404950725/nQJjzkens.png align="left")

### Ospfd daemon
The ospfd daemon controls routing tables using the Open Shortest Path First (OSPF) protocol. Because this implementation only supports OSPF version 2, it can only maintain IPv4 routing tables.

- To configure `ospfd` on `Quagga-suite` run the below command:

```bash
telnet localhost 2604
```


![telnet-ospfd.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660405000380/OqvWKUMjH.png align="left")

#### Ripd daemon
The Routing Information Protocol (RIP) daemon handles the routing tables of routers.

- The code below demonstrates how to configure `ripd` daemon in the Quagga suite.

```bash
telnet localhost 2602
```


![ripd.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660405041680/EfFiRv6yg.png align="left")

### Conclusion
The Quagga routing suite is primarily used to execute dynamic routing on the Linux platform. We learned how to install, configure, and use the Quagga routing suite in this tutorial.

The Quagga routing suite allows you to configure routing protocols like `OSPF` and `RIP` in a variety of ways. Small and medium businesses can benefit from using Quagga-based routing devices (SME).

Happy Coding!

#### Further reading
- [How to set up Squirrelmail on a Linux computer](https://squirrelmail.org/docs/admin/admin-3.html)

