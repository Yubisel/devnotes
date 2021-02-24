# Flameshot

[https://flameshot.org/key-bindings/](https://flameshot.org/key-bindings/)

##Install 

```bash
sudo apt install flameshot
```

You can easily configure your ```'print'``` keyboard shortcut to be assigned to Flameshot. Below an example to open Flameshot in GUI mode:

- Open ```Settings → Devices → Keyboard → Shortcuts```.
- Search for ```'print'```, and unbind the screen capture function by selecting it, and clicking ```backspace```.
- Scroll down and click on the ```'+'```.
- On ```'Name'```, name it ```'Flameshot'``` or ```'PrintScreen'```.
- Define the command as ```'flameshot gui'```.
- Select ```'Define shortcut...'``` and click your keyboard ```Prt Sc key```.

Now you can use your default keyboard key to launch Flameshot.

For defining multiple shortcuts you can repeat the process above, and just change the command.

Some examples of commands are:

```bash
# Capture a region using the GUI, and have it automatically saved to your pictures folder when clicking the save button in GUI
flameshot gui -p /home/user/Pictures
# Capture the active monitor and save it automatically to your pictures folder
flameshot screen -p /home/user/Pictures
# Capture the full desktop (all monitors) and save it automatically to your pictures folder
flameshot full -p /home/user/Pictures
```

## Usage

Example commands: - capture with GUI:

```bash
flameshot gui
```

Capture with GUI with custom save path:

```bash
flameshot gui -p ~/myStuff/captures
```

Open GUI with a delay of 2 seconds:

```bash
flameshot gui -d 2000
```

Fullscreen capture (asking savepath):

```bash
flameshot full
```

Fullscreen capture with custom save path (no GUI) and delayed:

```bash
flameshot full -p ~/myStuff/captures -d 5000
```

Fullscreen capture with custom save path copying to clipboard:

```bash
flameshot full -c -p ~/myStuff/captures
```

In case of doubt choose the first or the second command as shortcut in your favorite desktop environment.

A systray icon will be in your system's panel while Flameshot is running. Do a right click on the tray icon and you'll see some menu items to open the configuration window and the information window. Check out the information window to see all the available shortcuts in the graphical capture mode.

## CLI configuration

You can use the graphical menu to configure Flameshot, but alternatively you can use your terminal or scripts to do so.

Open the configuration menu:

```bash
flameshot config
```

Show the initial help message in the capture mode:

```bash
flameshot config --showhelp true
```

For more information about the available options use the help flag:

```bash
flameshot config -h
```
