# [Pico Ducky](https://github.com/dbisu/pico-ducky)

This is a simple clone of a Rubber Ducky, using a Raspberry Pi Pico, which
reduces the price substantially -- although this is at the price of speed of
both mounting and typing.

## Requirements

This requires a couple things:

- Raspberry Pi Pico (Pretty much any version)
- Circuit Python
- [pico-duky](https://github.com/dbisu/pico-ducky) files
- A Ducky Script[^2]

For the last one, you can use your own, or another premade script, like some of
the ones [Hak5](https://hak5.org/)
[lists](https://github.com/hak5/usbrubberducky-payloads) (The creators of the
[USB Rubber Ducky](https://hak5.org/collections/home2/products/usb-rubber-ducky)).

## Troubleshooting

### Re-Flash Pico

If you mess up flashing, or already flashed something else on it, and need to
reset the the flash memory, you can use
[pico-nuke](https://github.com/polhenarejos/pico-nuke).

All you have to do is put your pico into uf2 loading, by holding the BOOTSEL
button while reseting, or starting up, the pico. It will then show up as a new
drive. All you need to do this is copy the .uf2 file to the drive. You will
then see the led blink a couple times, which signals its completion. Then you
can flash what you please.

Broken down its:

<!-- Taken from <https://github.com/polhenarejos/pico-nuke> -->
1. Download the UF2 file corresponding to the version of your board from [Release section](https://github.com/polhenarejos/pico-nuke/releases)
2. Put your pico into uf2 loading (BOOTSEL+Reset).
3. Copy the .uf2 file to the mounted device (usually RP2-RPI).
4. Wait until the led blinks 3 times.
5. After completion, pico will enter again into uf2 loading.
6. Load your desired program (the other .uf2 file).

The commands should look something like the following:

```bash
# Download the following, if you have a `pico2 W` for example:
"github.com/polhenarejos/pico-nuke/releases/latest/pico_nuke_pico2_w-1.4.uf2O"
# Visit releases:
"github.com/polhenarejos/pico-nuke/releases/latest"
mount /dev/sdb1 /mnt
cp pico_nuke.uf2 /mnt/
```

Nice and simple.


[^2]: Ducky script, is a script create to create payloads for the USB Rubber
    Ducky, and is used in other knockoffs. You can use
    [this](https://dekunukem.github.io/duckyPad-Pro/doc/duckyscript_cheatsheet.pdf)
    cheatsheet, and [hak5's](https://hak5.org/)
    [official documentation](https://docs.hak5.org/hak5-usb-rubber-ducky/usb-rubber-ducky-by-hak5/).
