##uboot##

U-Boot 是一个主要用于嵌入式的引导加载程序，全称为 Universal Boot Loader 。可以想象成 Windows 上的 BIOS 。 
linux 系统启动时就必须要有一个 bootloader 程序，这段程序会先进行 DDR 等外设的初始工作。然后根据配置将 linux kernel 从指定的 flash 设备（QSPI Flash, SD, EMMC...,也可能通过网络）加载到板载的 DDR 进行启动运行。 
对于这个 bootloader 程序，目前有许多 bootloader 软件可以使用，例如 vivi, RedBoot, uboot。其中 uboot 在使用上最广泛 。 uboot-SPL 在 uboot 启动 kernel 的过程中，可能会遇到一个叫 uboot-SPL 的东西。
不是每个跑 linux 的芯片都会明确的有这个 uboot-SPL。可能是设计到了芯片的 bootRom 里面。具体也不是很清楚。

##uboot-SPL##

会被叫做二级boot，这里的二级是相较于 SOC 的 bootRom 而言。Soc 启动时最先执行的是 ROM 中的固化程序 （bootRom 程序）。 
uboot 虽然是为嵌入式系统而开发的，但其镜像一般都有几百 KB 。普通的 SOC 的片上 RAM 都无法完全容纳，所以一般需要一个程序来初始化片外的 RAM。加载并启动 uboot 。好在这个 SPL 程序一般都使用 uboot 自带的 SPL 程序。
在嵌入式使用中，无需将太多精力放在 uboot 上，它的目的就是为了启动 kernel。启动完之后便寿终正寝。

##linux kernel##

linux 内核，开源的电脑操作系统内核。在嵌入式中，有 FreeRTOS,UCOS 这些相较轻量级的操作系统。也有这种复杂但功能强大的操作系统。
在使用层面，嵌入式上使用 linux 。最重要的就是 kernel 中驱动的移植。一般都不会从一个纯净的 kernel 中去进行移植操作。而是芯片厂商提供的 kernel 基础上根据自己的板子进行各种外设驱动相关的适配工作。 
在 kernel 中做驱动开发是按照其规定的框架来编写驱动的，所以重点就是驱动框架的学习 dts dts 就是设备树，是用来根据板子不同来描述开发板上的设备信息的。老版本的 linux kernel 是没有设备树概念的。
后来因为 SOC 的发展，kernel 中需要对这些新增 SOC 的支持。而这些代码都会编译到 kernel 中。导致 kernel 日渐臃肿。后面就引入了在 PowerPC 等架构就已经采用的设备树。 

##rootfs## 

rootfs 即根文件系统，是 kernel 启动后挂载的第一个文件系统。rootfs 和 kernel 是分开的，但单独的 kernel 没有 rootfs 是没法正常工作的。 现在有许多制作 rootfs 的工具，如 busybox, buildroot,Yocto等。
其中 buildroot 中包含了 busybox 的功能，只需要简单的操作就可以生成一个 rootfs。十分方便。后续会去学习 buildroot 的使用。但若有时间想学习下 rootfs 的构建过程，可以使用 busybox 去慢慢构建 rootfs。
