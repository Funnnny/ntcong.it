+++
Categories = []
Tags = []
author = "Cong Nguyen"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
authorlink = "http://ntcong.it"
date = "2016-05-03T01:43:07+07:00"
thumbnail = "/img/avatar.png"
title = "Forensics:For2 Google's CTF Writeup"

+++

I just started my journey in information security for a while, my forensic skills is some what non-existent, so I'm pretty excited when I can solve a decent forensic problem in a CTF (that's why I need to write about it right away).  
So, I can solve some Web challenges, a plaintext IRC log extraction, and move myself up in the scoreboard a little. That's when I tried to see the challenge other people around me were solving. And I think maybe I can solve this (it helps a lot since I thought about giving up several times, also a problem I need to fix myself). This is how a newbie solve the problem.  

The challenge is [here](https://capturetheflag.withgoogle.com/challenges/forensics). It's a simple pcap file, Wireshark tells us it's USB protocol, I looked down a bit and saw a "GET DESCRIPTOR" request/response, it said "Logitech Optical Mouse". It's must be an USB mouse then.  
The rest of the traffic is USB Interrupt, so the mouse is doing something. I used Python with dpkt to read the pcap file and extract the information.
{{<highlight python>}}
with open(sys.argv[1], 'rb') as f
  pcap = dpkt.pcap.Reader(f)

  for _, buf in pcap:
      irp = ''.join((buf[:10]))
      if binascii.hexlify(irp) != '1b0060fc790280faffff':
          continue
      print binascii.hexlify(buf[27:])
{{</highlight>}}

irp is the USB pcap header length plus IRP ID, the USB Interrupt packet has length equals 1b00. The raw data is in this form: "00(or 1) aa bb 00", I search for USB mouse data and found [this](http://www.usbmadesimple.co.uk/ums_5.htm). So first 8 bits are the mouse 1-5 button, two next 8 bits are for xx and yy movement, and last 8 bits are mouse wheel.  
There's only 00 and 01 for first 8 bit, 01 is a left click (or whatever). We just need to track the mouse movement, record when there's a click. X and Y coordinate is straightforward: value higher than 128 mean it's moving left/down, lower than 128 mean it's moving right/up. You don't need to be right about left/down right/up, just make sure you define a rule and flip/rotate the result as needed.

{{<highlight python>}}
def move(vel):
    vel = int(vel, 16)
    return vel if vel < 127 else vel - 256

with open(sys.argv[1], 'r') as f:
    xx = 0
    yy = 0
    for line in f:
        click, x, y, _ = line.strip().split(":")
        xx += move(x)
        yy += move(y)
        if click == '01':
            print(xx, yy)
{{</highlight>}}

Run this and we will get the list of click coordinate. Let's get this on a graph, real man uses gnuplot: 

    gnuplot -e "set output 'click.png';plot 'click_coordinates.txt'"
    
My default gnuplot terminal is png, the result image is flipped, and it's easy to fix

![Result](/img/for2/click.png)

Poor cat!
