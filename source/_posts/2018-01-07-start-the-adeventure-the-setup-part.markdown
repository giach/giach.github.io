---
layout: post
title: "Start the adeventure: The Setup Part"
date: 2018-01-07 12:22:26 +0200
comments: true
categories: outreachy
published: true 
---

## Intro

{% img right /images/yoda-tux.jpg 'image' 'images' %} 

Our wise friend, will introduce us the world of Linux Kernel. Check below the piece of wisdom from Yoda Tux.

Good coding vibe needs a good working environment. So, our setup, let’s see how to do.  There are several simple steps and, take them one by one, will we.  Hihihi!

<a href='https://ro.pinterest.com/pin/270356783857623037/'>Link to the Yoda Tux image</a>


<br> 
## The source code
Download the kernel source tree of Industrial I/O:

`git clone https://www.kernel.org/pub/scm/linux/kernel/git/jic23/iio.git`

## The default config
The next step is to configure the kernel. As a good practice, use the default configuration and later add or remove kernel modules. To create the configuration file, run the following command:

`make defconfig`

The configuration is based on the architecture type of the host system (eg: x86, powerpc, sparc64). The available architectures are to the iio/arch path.

## Customize the kernel config 
As the I/O subsystem is not part of the default configuration, we have to add the module using the command:

`make menuconfig`

This command opens the menu from the image, which is not friendly, but easy to use. Navigate with the arrows keys, press Y to include a module, press N to exclude a module and M to create a module from that feature/driver.

{% img center /images/menuconfig.png 'image' 'images' %}

To search for any feature or module, just press the **/** (the slash key from the keyboard) and type the name. 


{% img center /images/search-bar.png 'image' 'images' %}

At this moment, we are looking for the Industrial I/O Subsystem. Let’s type IIO and press Enter. The result looks like the below image.

{% img center /images/iio-result.png 'image' 'images' %}

Each results from the list has a corresponding number between **parentheses**. Go to the feature by pressing the indicated number. 

In this situation, the Industrial I/O subsystem has the number **(1)**, but it depends on other 3 features:
*RTC_DRV_HID_SENSOR_TIME*
*RTC_CLASS*
*USB_HID*

The first one is marked with **[=n]**, which means that it is not selected to be part of the kernel configuration. Adding this features includes automatically the Industrial I/O module in the kernel. So, let’s search for the *RTC_DRV_HID_SENSOR_TIME* and include it.  Further, search again for the IIO. Check if the feature is marked with **[=y]**. 

Save the new kernel configuration by pressing the **save** button. Keep the default name of the configuration file which is **.config**. You can check the existence of this file in the **iio** folder (the root folder of the source code).

## Compile the source code
The next step is to compile the kernel. It is recommended command to use all the processing units in order to finish the compiling fast. In the next command, the **nproc** gives the maximum number of the processor units available on your hardware. So, take a break, have some coffee or tea and come back later. 


``make -j `nproc` ``


In case you still want to use the computer during the processing time, replace the **nproc** with a number lower than the maximum units.

In case a feature was marked as module in the menuconfig, the module (a file with he **.ko** extension) will be available when the compiling is finished. 

## The kernek image
After the compiling is done, the kernel image could be found on the path:

`iio/arch/your_architecture/boot/bzImage`

In my case, the path is **iio/arch/x86_64/boot/bzImage** because the kernel was compiled for a 64bit Intel architecture. 

## Qemu setup

The Qemu is an easy and fast tool to create a virtual machine. Follow the tutorial to build a qemu with a custom kernel (<a href='https://mudongliang.github.io/2017/09/12/how-to-build-a-custom-linux-kernel-for-qemu.html'>link</a>) and use the kernel image that you have just compiled. 

My piece of advice is to run the qemu as in the tutorial, with 

`-nographic -append "console=ttyS0"`

These two parameters let the qemu to run without opening a new windows and to show the output in the working terminal.


## Possible issue:
The qemu is on 32bit, but the kernel is compiled on 64bit. Check in the menu config if the first option matches the type of the qemu architecture.
{% img center /images/arch-type.png 'image' 'images' %}

## Time to work

In case you encounter a problem with the tutorial, send me an email to 

<georgiana.chelu93 AT SPAMFREE gmail DOT com> 

and as Yoda Tux would say:

<blockquote>
Patience you must have, my young padawan.
</blockquote>
