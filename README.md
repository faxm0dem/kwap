# Keyboard sWap

This little utility lets you swap the internal keyboard and your external one automatically.
It is intended to integrate into `udev` so when plugging in your external keyboard, the internal is disabled automatically in Xorg.
The idea behind this tool was to be able to place my Atreus keyboard on top of my laptop's internal keyboard.

## Synopsis

```
kwap --external=15 --xauthority /home/me/.Xauthority
```

You can find out what IDs to use in the options by running `xinput list`:

```
☠ xinput list --short
⎡ Virtual core pointer                          id=2    [master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
⎜   ↳ ELAN Touchscreen                          id=11   [slave  pointer  (2)]
⎜   ↳ DLL07A0:01 044E:120B                      id=12   [slave  pointer  (2)]
⎜   ↳ DualPoint Stick                           id=13   [slave  pointer  (2)]
⎜   ↳ Keyboardio Atreus                         id=14   [slave  pointer  (2)]
⎜   ↳ Keyboardio Atreus                         id=15   [slave  pointer  (2)]
⎜   ↳ Kensington      Kensington Expert Mouse   id=17   [slave  pointer  (2)]
⎣ Virtual core keyboard                         id=3    [master keyboard (2)]
    ↳ Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
    ↳ Power Button                              id=6    [slave  keyboard (3)]
    ↳ Video Bus                                 id=7    [slave  keyboard (3)]
    ↳ Power Button                              id=8    [slave  keyboard (3)]
    ↳ Sleep Button                              id=9    [slave  keyboard (3)]
    ↳ Integrated_Webcam_HD                      id=10   [slave  keyboard (3)]
    ↳ Keyboardio Atreus                         id=16   [slave  keyboard (3)]
    ↳ Intel HID events                          id=18   [slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard              id=19   [slave  keyboard (3)]
    ↳ Dell WMI hotkeys                          id=20   [slave  keyboard (3)]
```

The `--internal` would be in this case `id=3` which is set by default to the `id` corresponding to the line `Virtual core keyboard`.
The `--external` would be your external keyboard, *e.g.* `id=16` corresponding to the `Keyboardio Atreus`.
The other two you can get using your environment variables `$XAUTHORITY` and `$DISPLAY`.

You can then add the following rule to your `udev` rules folder:

```
ATTRS{idVendor}=="1209", ATTRS{idProduct}=="2303", RUN+="/path/to/kwap --external=15 --xauthority=/home/me/.Xauthority"
```

You can find about the `idVendor` and `idProduct` values corresponding to your external keyboard using the `lsusb` command:

```
☠ lsusb
[...]
Bus 003 Device 008: ID 1209:2303 Keyboardio Atreus
```
