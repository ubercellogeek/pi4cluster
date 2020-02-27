# Yet Another Raspberry Pi 4 Cluster

My attempt at designing a simple and clean Raspberry Pi 4 cluster.

![Cluster](https://i.imgur.com/d3dpUfE.jpg)

More pictures: [side](https://i.imgur.com/c2pA6ft.jpg), [fan](https://i.imgur.com/MvKaNwb.jpg), [back](https://i.imgur.com/nm6leMx.jpg)

## Yeah, but why?

I've spent the better part of my 15 years as a software engineer under the .NET umbrella. The past 3 years or so almost exclusively working with .NET Core. It's an exciting time to be a .NET developer. You can now run your C# code on a tiny little arm64 based SOC. Not only that, but you can run your C# code inside a docker container that's being managed by a Kubernetes cluster that is in-turn running on a group of tiny little arm64 based SOCs. I wanted to see what that's like. Turns out, it's pretty freaking fantastic. 

The only problem: I couldn't find any really clean physical cluster designs that could support mounting options for SSDs. I will admit, my inspiration came from this design [here](https://www.thingiverse.com/thing:3858968). 

**But, why do you *need* SSDs!?** I don't. I totally don't. You can run this cluster off of the SD cards alone. But, you'll find that you only get around ~30MB/s of disk i/o performance. Since the Raspberry Pi 4 has support for USB3 and up to 4GB of RAM I wanted to see what disk i/o numbers I could achieve by using a cheap SSD as the main OS drive. Turns out, it's about 200MB/s.

## Files

I've included the standard `.stl` model files as well as the `.f3d` Fusion360 files in case anyone would like to modify what I've created. I write code by day, so my 3d modeling skills are next to null. I make no promises as to the quality of the design. Files are located in the `design` folder.

I've also included the Ansible scripts I used to get the whole cluster bootstrapped. It's a fork of [jduncan-rva's wonderful Ansible playbook](https://github.com/jduncan-rva/kube-pi-lab). All credit goes to him for that work.

## Design

As stated above, I wanted to be able to physically mount the SSDs to the cluster.

The other difference between this design and most of the others you'll find on the internet is that the Raspberry Pis are powered via POE. This alleviates the need for an additional usb power distribution brick. It does however increase the overal cost due to the need for each board to have a POE HAT installed.

## Parts

*Cheap* is a relative term, right?

| Qty | Part | Price (USD)
--- | --- | ---
4 | [Raspberry Pi 4 (4GB)](https://www.canakit.com/raspberry-pi-4-4gb.html?cid=usd&src=raspberrypi) | $55.00
4 | [Crucial 240GB SSD](https://www.amazon.com/gp/product/B07G3YNLJB/ref=ppx_yo_dt_b_asin_title_o00_s01?ie=UTF8&psc=1) | $23.99
4 | [POE HAT](https://navolabs.com/product/raspberry-pi-4-and-3-micro-poe-hat/) | $37.00
4 | [LaCie E481033 ASM1153 USB Type C SSD Controller](https://www.ebay.com/itm/LaCie-E481033-ASM-1153-REV-A0-2-5-SATA-HDD-USB-Type-C-Controller-PCB/372685968219?ssPageName=STRK%3AMEBIDX%3AIT&_trksid=p2057872.m2749.l2649) | $6.98
4 | [USB Type C Cable - 90 Degree](https://www.amazon.com/gp/product/B07LBG18W6/) | $7.99
4 | [SanDisk Ultra 16GB Micro SD](https://www.amazon.com/gp/product/B073K14CVB) | $5.99
1 | [Noctua NF-F12 5V 120MM Fan](https://www.amazon.com/gp/product/B07DXLV5Z6) | $19.00
1 | [Raspberry Pi Aluminum Heatsink Kit](https://www.amazon.com/gp/product/B07217N5LS) | $7.99
1 | [tp-link TL-SG1005P 5-Port POE Gigabit Switch](https://www.amazon.com/TP-Link-Compliant-Shielded-Optimization-TL-SG1005P/dp/B076HZFY3F) | $44.99
1 | [5-Pack Slim-Run 1ft Cat6a cables](https://www.amazon.com/gp/product/B01BGV2C7U) | $8.99
1 | [M2 Screw Kit](https://www.amazon.com/gp/product/B07NKNCM8Q) | $10.99
2 | [M3 Screw and Standoff Kit](https://www.amazon.com/gp/product/B01HDR72Q2) | $11.99
1 | [M3 M4 M5 Socket Cap Nuts and Bolts](https://www.amazon.com/gp/product/B071XNBGRQ) | $14.99

## Assembly/Prints

Lots of standoffs and M2/M3 screws. M2.5 screws are used to hold the individual Raspberry Pi boards to their base mounts. M2 screws fasten the USB C/SSD adapters to the caddies. Everything else uses M3 screws/bolts/nuts/standoffs. Everything needed should be in the parts list.

The 3d printed parts were produced via a Monoprice Maker Select v2 running Marlin firmware (v3). I used Cura to slice the models. 

* Amazon Basics PLA (black)
* 0.2mm layer height

## Helpful Links

This build guide is mainly about the physical build of the cluster. However, if you're interested on how to get it actually up and running as a Kubernetes cluster I found the following links extremely useful.

* [Kube-Pi-Lab](https://github.com/jduncan-rva/kube-pi-lab) (this is the one I used to bootstrap my entire cluster with a one-liner)
* [K8s Pi - Production-ish Kubernetes cluster](https://github.com/ljfranklin/k8s-pi)
* [Setting up the Raspberry Pi 4 to boot from a USB3-attached SSD](https://jamesachambers.com/raspberry-pi-4-usb-boot-config-guide-for-ssd-flash-drives/)

## Additional Information

The cluster as it is (sitting next to me on my desk as I write this), is running the following:

* Kubernetes 1.17 (one master node, 3 worker nodes)
    * Calico CNI
    * MetalLb
    * ingress-nginx
* Each node is running Ubuntu 19.10 (arm64) (bootloader on the SD, OS on the SSD)
* GlusterFS (each node is a part of the quorum and exposes an LVM block device located on the SSD as a brick)
* Heketi (master node only)


I'd love to get feedback on what I've done here and any suggestions for how to improve. Give it a star, or better yet, submit a PR!

















