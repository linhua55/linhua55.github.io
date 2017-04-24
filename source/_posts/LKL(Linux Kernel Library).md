# LKL(Linux Kernel Library)

## Golang program with LKL

### issue

Golang has no Glibc dependency, it calls linux's syscalls directly. Thus golang program compiled with gc(normal go compiler) can't be hijacked by liblkl-hijack.so.

### Two methods

1. add LKL arch to golang. (add LKL syscalls)
2. use gccgo to compile the golang program.  As program compiled by gccgo will use glibc to call syscalls.

Compiling golang program(v2ray) with gccgo needs some modifications. But the program compiled with gccgo will occupy huge memory than normal v2ray. 

v2ray(gccgo): 8% memory

v2ray(gc)(normal): 2% memory

## raw/packet socket backend for LKL in openVZ

OpenVZ's venet0 network interface is a Cooked interface. Its raw packet has no MAC layer(14 bytes).

It can't use  `AF_PACKET/SOCK_RAW`, can only use `AF_PACKET/SOCK_DGRAM`.

And thus, It has no ARP protocol packet, only has IP protocol packet. How about IPSEC?(I don't know.)

It don't need gateway, it's a pointopoint network interface.

LKL has a native support for raw/packet socket backend. But it's not for openVZ, it need send ARP packet. While, we can modify some pieces of code to suport raw/packet socket in openVZ.

### Test send/recv with AF_PACKET/SOCK_DGRAM

As buffer is a form of `struct iovec`. Thus can only use `writev/readv` or `sendmsg/recvmsg`. And as the socket type is `SOCK_DGRAM`, It's connectionless, you must specify the destination address, thus can't use `writev/readv`, but can use `sendmsg/recvmsg`.

### The setting of LKL's network interface(eth0)

We can't use original setting of LKL's network interface. As it for network with a gateway. But openVZ has no gateway and we also can't set a tun/tap interface to act as a gateway for LKL. We can set `eth0`(LKL's network interface) to `POINTOPOINT` mode, just like the openVZ's `venet0` interface. But haven't find a method to set `eth0` to `POINTOPOINT` mode. Surprisingly, we can set `eth0` interface to `NOARP` mode. And it indeed work just like wanted.

You can test it(the `NOARP` method) with `ping` program(without LKL) on your linux box. Remember to use `tcpdump` to monitor the interface. First, you set your network interface without a gateway, and you `ping`, you can't find output of `tcpdump`. But when you set your interface to `NOARP` mode, you `ping`, and it will work(the `tcpdump` program will output something ). 

the tcpdump command:

    $ sudo tcpdump -nnq -i your-interface icmp

### The `struct iovec` type buffer in LKL

LKL's  rx function's `iovec *` iov pointer  array's  first  `iovec` buffer is useless. You should only use the second buffer `iov[1]`.

### TODO

If we use TCP with LKL(raw socket), it will fail. As LKL's network stack will return `RST` packet to peer to cutoff the connection. Thus in order to distinguish the LKL  network stack with native network stack, we should workout on it.

One workaround is to use LKL for `listen socket(server port)`, not for client socket. 

