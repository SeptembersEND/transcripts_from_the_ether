# [Bad USB](https://en.wikipedia.org/wiki/BadUSB)

A Bad USB is a malicious piece of hardware, that acts as a HID (Human Interface
Device), or keyboard, and can be programmed to execute malicous software. This
enables the usb to execute automated commands and keystroks.

This attack can be mitigated, although not easily. You can disable the port,
either physically, in BIOS, or in software, or you can use some other
software[^1] to limit the maximum typing speed.

## [USB Rubber Ducky](https://shop.hak5.org/products/usb-rubber-ducky)

A USB Rubber Ducky is a type of Bad USB, that uses a custom scripting language,
called [Ducky Script](https://shop.hak5.org/pages/duckyscript-3-0), to create
payloads, which dictate what is typed and how.


[^1]: For some further, although probably unnecessary remediations, you can look
    at [Pedro M. Sosa's](https://github.com/pmsosa) program
    [duckhunt](https://github.com/pmsosa/duckhunt).

    There is also some good suggestions, although now old, on an old
    [Information Stack Exchange](https://security.stackexchange.com/) question,
    "[How to stop USB Rubbery
    Ducky?](https://security.stackexchange.com/questions/173869/how-to-stop-usb-rubber-ducky)"
    Some suggest creating a blacklist policy, or even completly block all USB
    Human Interface Devices.

    On linux, you can use the program [USBGuard](https://usbguard.github.io/),
    which allows for setting whitelist and blacklist capabilities, based on
    device attributes. More on [Arch
    Wiki](https://wiki.archlinux.org/title/USBGuard),
    [debian](https://wiki.debian.org/USBGuard),
    [redhat](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/sec-using-usbguard)
    and more.

    There are probably even more solutions, but they may not be necessary,
    especially since a USB Rubber Ducky "requires physical access" (except when
    it does not, for example
    [duck2python](https://github.com/CedArctic/ducky2python)), and then you have
    worse problems.

