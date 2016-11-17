+++
Categories = []
Tags = []
author = "Cong Nguyen"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
authorlink = "http://ntcong.it"
date = "2016-09-12T10:16:11+07:00"
thumbnail = "/img/avatar.png"
title = "Network Interrupts and How It Kills Your Server"
draft = true

+++
Two years ago when I was in a SA class, I talked about interrupt storm and how can it kill your server. Some people asked me about the detail technical aspect of how it happens. I think this is a very interesting topic and maybe I should pay my debt.

## Preface ##
The problem only occur in a very high performance networking setup. In my case it's a server with two 10Gbps card. If your server only have 2 or 4 1Gbps card, there might be some problems, but generally it's because of unoptimized configuration.  
I worked on a DDOS-mitigation system, that's why I receive a lot of abnormal traffics, which will put more workload in the system. So if you have a caching server with 2 10Gbps, YMMV.  
Linux networking stack is young, many iptables/netfilter path optimization only happened in 2014-2015. Most of the time if you asked people about Linux networking stack, they will just tell you "it's slow". In contrast to FreeBSD, which is using for a long time by ISPs, even its toolings are superior to Linux.

Now it's 2016, receiving million of packets per second is not a problem for Linux anymore, but let's first take a look at why send/receive packet in Linux is so slow.

## Interrupts ##

Interrupts are core part of how a processing unit work (either a hardware like CPU, or a software like kernel). Once the CPU receives an interrupt, it will stop the execution, save its context, call the interrupt handle, after that restore its context and then resume the execution. Linux kernel also uses interrupts to handle its processing, called softirq. Nowaday many drivers and core part of the kernel don't use softirq anymore (correct me if I'm wrong), the network still uses it.

Everytime you receive a packet, the network card will issue an IRQ (Interrupt Request) to the CPU, the CPU will in turn copy the packet to the memory, and uses netfilter to process it. I will address this after.  
Interrupts is very costly, each interrupt costs about 200 to 2000 instructions. So for a famous i5-2500k Intel running at 83G instruction per second, each interrupt is about 12ns.

## Optimizing Interrupts ##

Before jumping on some calculation about the challenge of processing a network packet, let's optimize the interrupt handler so it won't die easily.  
By default Linux will process all your IRQ in a single CPU core. A few years ago most Linux distro had irqbalance, which will distribute irq across all CPU cores.

But sadly, it's not enough. irqbalance tries to distribute the IRQ across cores, which sometime is not what we wanted: a RX queue receives a lot of packets will scatter over all cores, reducing performance. Nowaday most network cards have multiple RX queue, imagine it's like having multiple network card, the packet from same IP or same target will be sent in a specified queue.  
In order to maximize the performance, we need to bind each RX queue to a CPU core, this can be done in a simple script:
{{< highlight bash >}}
  (let CPU=0; cd /sys/class/net/eth0/device/msi_irqs/;  
    for IRQ in *; do
      echo $CPU > /proc/irq/$IRQ/smp_affinity_list
      let CPU+=1
    done)
{{< /highlight >}}

## The problem with 10Gbps ##
With the rise of 10Gbps network card, the interrupt problem is getting harder to solve. Let's say you receive a large number of DNS respond, around 10G/8/250 = 5M pps. With a i5-2500k@3.3Ghz and 4 cores, you only have 3.3G\*4/5M = 2.5k cycle to process each packet, or 1s\*4/5M=800ns.

Seems like a lot, but 1 interrupt already costs you 12ns, let's get this very simple and assume each iptables rule only cost 1 interrupt (ignore its processing time), and in a perfect world you can have like 30 iptables rules and lose half of your processing power. And still each cache miss costs around 30-50ns, a lock/unlock costs 16ns, a syscall costs 70ns, leaving you very very little room to process. Your server probably won't have like 50% idle time reserved for networking. Don't let me talk about 2 10Gbps cards.

So, an interrupt storm is when you receive a lot of network packets, overload your system with interrupts. When this happens, your server won't response to keyboard, IO or anything.


