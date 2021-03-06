Xenguest Manager
================

Introduction
------------

xenguest-manager is a tool to manage Xenguest images generated by
[xenguest-mkimage](xenguest-mkimage.md).

On a Xen Dom0 system it will:
- create a xen guest from a xenguest image: extract its components, create a
  disk for the guest using LVM volumes.
- start/stop a xen guest (during init or using xenguest-manager directly).
- check guest status

xenguest-manager is composed of 2 shell scripts:
- xenguest-manager which can be used from command line to start/stop/check
guests and create or remove guest using xenguest images.
- xenguest-init which is called during init to automatically create and start
some guests as part of the host init process.

Usage
-----

xenguest-manager must be called like this:
`xenguest-manager OPERATION [OPTIONS]`
The following operations are available:
- create XENGUEST_IMAGE [GUESTNAME]: create a guest from a xenguest image file
  as guest GUESTNAME. If GUESTNAME is not given the image file name is used
  without the xenguest extension.
- remove GUESTNAME: remove the guest GUESTNAME.
- start GUESTNAME: start the guest GUESTNAME.
- stop GUESTNAME: stop the guest GUESTNAME (this is using `xl stop` which is
  sending a stop signal to the running guest).
- kill GUESTNAME: force stopping the guest GUESTNAME.
- list: list the available guests.
- status [GUESTNAME]: print the current status of GUESTNAME. If GUESTNAME is
  not given, print the status of all guests.

For a detailed help on available options please use:
`xenguest-manager --help`

Bitbake parameters
------------------

Several parameters are available to configure the xenguest manager during Yocto
project compilation (those can be set in your project local.conf, for example).

The following parameters are available:

- XENGUEST_MANAGER_VOLUME_DEVICE: This is the device path used by the 
  xenguest-manager on the device to create LVM disks when guests have a disk
  configuration.
  This is set by default to "/dev/sda2".

- XENGUEST_MANAGER_VOLUME_NAME: This is the LVM volume name that the 
  xenguest-manager will create and use to create guest LVM disks.
  This is set by default to "vg-xen".

- XENGUEST_MANAGER_GUEST_DIR: This is the directory on Dom0 where the 
  xenguest-manager will look for xenguest images to create during init. That's
  the place where xenguest images can be added to have them automatically
  created during next Dom0 boot. The xenguests found there will only be created
  if they were not already before (the basename of the files is used as guest
  name).
  This is set by default to "/usr/share/guests".

