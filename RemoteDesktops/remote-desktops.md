# What this is

This is a brief overview of multiple methods to get a remote desktop on
a headless server. There are multiple variables to pay attention to:
the protocol or the method of getting graphics; and how the client connects
(most notable being clientless[^1]).


## Software

This is some of the remote desktop software, from servers to clients, that
exists.

<!-- TODO: add License column-->
| ***Software*** | ***Protocol*** | ***Type*** | ***Platform*** |
|---|---|---|---|
|---|---|---|---|
| Remote Desktop | RDP | Client | Windows |
| FreeRDP | RDP | Client | Windows |
| [Xrdp](#xrdp) | RDP | Server | Xorg |
|---|---|---|---|
| RealVNC | VNC | Client | Mobile |
| RealVNC | VNC | Client/Server | Win/Xorg/MacOS|
| TigerVNC | VNC | Client | Mac |
| TigerVNC | VNC | Client/Server | Win/Xorg |
| TigntVNC | VNC | Client/Server | Win |
| TurboVNC | VNC | Client | Win/Xorg/Mac|
| TurboVNC[^3] | VNC | Client/Server | Xorg |
| [x11vnc](#x11vnc)[^4] | VNC | | |
| X2vnc | VNC | ~Server | Xorg |
| wayvnc | VNC | Server | Wayland |

For multiple protocol software:

| Software | Protocols | Type | Platform |
|---|---|---|---|
|---|---|---|---|
| Virt-Viewer | SPICE,VNC | Client | Xorg |
| Remmina | RDP,SPICE,VNC | Client | Xorg |


## Lore

I first needed to fall down this rabbit hole, when I wanted to create a
graphical dockers image for testing. Then enventually need to create
one for a local server.[^5]

So this has become my personal repository of information on this matter.


# [RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol)

RDP is a popular remote desktop protocol, developed by Microsoft.
There are now open-source versions of this protocol, for example
[FreeRDP](https://www.freerdp.com/) (a fork of
[rdesktop](https://en.wikipedia.org/wiki/Rdesktop)).

RDP uses a client to connect to a RDP server. So you will need both a client
and a server to get this working, unless you choose to use clientless.

There are many differences between RDP and VNC, but it boils down to RDP being
more advanced, for example: compression takes advantage of font knowledge and
tracking of window states, audio redirection, file system redirection, and
more.

For a list of features:

<https://en.wikipedia.org/wiki/Remote_Desktop_Protocol#Features>

## Clients

### Remote Desktop

This viewer is installed by default on Windows Pro, and is a Windows exclusive.
It does make it easy for those who have access, to connect to RDP servers on
other servers.

To use it all you need to do is to search for *Remote Desktop Connection* and
type in the ip address, or hostname, and press connect. You can also access by
using *Win+R* and typing *"mstsc"*.

## Servers

### Xrdp

"***Xrdp*** is a daemon that supports Microsoft's Remote Desktop Protocol (RDP)."[^2]
*Xrdp* can use two forwarding modes: *Xvnc*, uses a VNC server; or
*xorgxrdp*/*X11rdp*, which communicates directly with the X server.

To install do the following:

```bash
# On debian, package:
# <https://pkgs.org/download/xrdp>
apt-get install xrdp

# RPM, package:
# <https://pkgs.org/download/xrdp>
yum install xrdp
dnf install xrdp

# Archlinux, install xorgxrdp to use that forwarding mode.
# Wiki: <https://wiki.archlinux.org/title/Xrdp>
pacman -S xrdp xorgxrdp

# OpenSUSE, guide:
#<https://support.scc.suse.com/s/kb/Configuring-xrdp-for-FIPS-compliance>
zypper install xrdp
```

#### In chroot/docker

You can also run this in a docker image. To do this you will either have to get
systemd to work within the environment, or start it yourself. We will focus on
the second one.

To start an xrdp server manually, you will need to run the following command.
You could also use the *init.d* directory, to start it like a service, if it is
present:

```bash
# Use command directly, in bash
xrdp-sesman && xrdp
## You can stop it like this
pkill xrdp
pkill xrdp-sesman

# Start service manually
/etc/ini.d/xrdp start
/etc/ini.d/xrdp stop
```

#### Connect to existing Xorg session

By default xrdp creates a new Xorg session when the server is started. This may
not be what you would want. One way to connect to an existing X session is by
using the x11vnc backend. This does require, however, the use of X11vnc.

You can use x11vnc directly with the following command:

```bash
x11vnc -nocdamage -display :0 -safer -once
```

You will also need to change xrdp's configuration file, in
*/etc/xrdp/xrdp.ini*, to something like the following:

```
# https://github.com/neutrinolabs/xrdp/issues/960
[xrdp1]
name=X11vnc
lib=libvnc.so
ip=127.0.0.1
port=5900
username=ask
password=ask
```

And also add the following to the sesmon config, in */etc/xrdp/*:

```
# https://github.com/neutrinolabs/xrdp/issues/960
[X11vnc]
param=x11vnc
param=-noxdamage 
param=-display
parasm=:0
param=-safer
param=-once
```

If you want you can also set x11vnc in systemd unit file on boot:

<https://gist.github.com/lightrush/e6c3310fd0795b03f19daa40abf674e1>


# VNC

[VNC](https://en.wikipedia.org/wiki/VNC) is a very simple desktop-sharing
system that uses the Remote Frame Buffer
([RFB](https://en.wikipedia.org/wiki/RFB_(protocol))) protocol.

VNC can use a lot of bandwidth, due to its simple design, and as such various
methods have been devised to reduce the overhead. Tightvnc provided a
comparison on their site:

<https://www.tightvnc.com/archive/compare.html>

## Servers

### x11vnc

To install x11vnc you can do it automatically (especially if you want some more
advanced features), or manually.

The normal installation of x11vnc is increadibly easy, just download using your
package manager:

```bash
# Debian package:
#   <https://pkgs.org/download/x11vnc>
apt-get install x11vnc

# RPM package:
#   <https://pkgs.org/download/x11vnc>
yum install x11vnc
dnf install x11vnc
```

You can also use [domomg's](https://github.com/domomg)
[x11vnc-setup](https://github.com/domomg/x11vnc-setup) script. It is also good
to read to learn the commands for different distros for setup.

- <https://github.com/LibVNC/x11vnc>
- <https://en.wikipedia.org/wiki/X11vnc>
- <https://libvnc.github.io/>


# Clientless

There are a couple of way to use clientless vnc. Here is a list:

<!-- TODO: add License column-->
| ***Software*** | ***Description*** | ***Protocols*** |
|---|---|---|
|---|---|---|
| noVNC | open-source VNC browser client | VNC |
| Apache Guacamole | clientless remote desktop gateway | VNC,RDP,SSH |


## noVNC

[noVNC](https://novnc.com) is a cool open-source clientless VNC "client library and application". There
are many [projects and
companies](https://github.com/novnc/noVNC/wiki/Projects-and-companies-using-noVNC)
that use this in their products and projects.

To install and use *noVNC* there are a couple paths to take. You could 1) use
the snap image, 2) install via npm (*npm install @novnc/novnc*) or 3) download
and run manually, which can be done as follows:

```bash
# Download repository
git clone https://github.com/novnc/noVNC
cd noVNC

# Run script
#   vnc address: mostlikely `localhost`
#   listen address: (127.0.0.1) for local. (0.0.0.0) for any.
./utils/novnc_proxy --vnc <vnc address>:<port> --listen <listen address>:<port>
```

It does require of course that you have another vnc server running, so take
your pick.




[^1]: Clientless remote desktop software allows the client to connect
    to a server without a deticated client. Most notably this is done
    by creating a webserver. A very popular example is
    [Apache Guacamole](https://en.wikipedia.org/wiki/Apache_Guacamole).

[^2]: You can find more information on
    [Xrdp](https://github.com/neutrinolabs/xrdp) from there site,
    <https://www.xrdp.org/>, or from other 

[^3]: You can find *OS Support* here,
    <https://turbovnc.org/Documentation/OSSupport>, and *Window Manager*
    compatibility information here,
    <https://turbovnc.org/Documentation/Compatibility32>.

[^4]: Currently unmaintained as of writing, <https://github.com/LibVNC/x11vnc/issues/186>.

[^5]: You can put systemd in a docker container, like shown here
    <https://spectrelabs.io/research/systemd-systemctl-inside-docker>. It seems
    to be much more difficult to add systemctl chroot.
