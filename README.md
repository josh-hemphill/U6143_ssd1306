# U6143_ssd1306

Display driver for UCTRONICS Ultimate Rack with PoE Functionality for Raspberry Pi 4 (SKU U6145)

## I2C

Begin by enabling the I2C interface:

```bash
sudo raspi-config
```

Choose `Interface Options` | `I2C`, then answer `Yes` to whether you would like the ARM I2C interface to be enabled.

(On a freshly flashed Rasbian OS SD card, you can edit the `config.txt` file and uncomment `dtparam=i2c_arm=on` to enable I2C instead)

## GitHub releases available now with pre-built binary and installer

You can now install by downloading the latest `.deb` installer and it will automatically enable and start the background service.

```bash
wget https://github.com/josh-hemphill/U6143_ssd1306/releases/download/latest/display-u6143.deb`
sudo dpkg -i display-u6143.deb
```

You can optionally set the DISPLAY_CYCLE_TIME_S environment variable to control the cycle time of the display
Either in the global env locations or in the service file: /lib/systemd/system/display-u6143.service

```ini
[Service]
Environment="DISPLAY_CYCLE_TIME_S=5"
```

Or if you just want to run the binary yourself:

```bash
wget https://github.com/josh-hemphill/U6143_ssd1306/releases/download/latest/display-binary`
chmod +x display-binary
# You can optionally set the DISPLAY_CYCLE_TIME_S environment variable
# to control the cycle time of the display
sudo DISPLAY_CYCLE_TIME_S=5 ./display-binary
```

## Clone U6143_ssd1306 library

```bash
git clone https://github.com/darkgrue/U6143_ssd1306.git
```

Or if you don't have git installed (it's not by default)

```bash
curl -sSL https://github.com/darkgrue/U6143_ssd1306/archive/refs/heads/master.zip -o temp.zip && unzip temp.zip && rm temp.zip
```

## Compile

```bash
cd U6143_ssd1306/C
```

```bash
make clean && make 
```

## Run

```bash
sudo ./display
```

## Add automatic start script

Copy the binary file to `/usr/local/bin/`:

```bash
sudo cp ./display /usr/local/bin/
```

Choose one of the following configuration options (`systemd` **or** `rc.local`):

```bash
sudo cp ./contrib/U6143_ssd1306.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable U6143_ssd1306.service
sudo systemctl start U6143_ssd1306.service
```

**OR** add the startup command to the `rc.local` script (not recommended)

```bash
sudo nano /etc/rc.local
```

and add the command to the rc.local file:

```bash
/usr/local/bin/display &
```

Reboot your system:

```bash
sudo reboot now
```

## For older 0.91 inch LCD without MCU

For the older version LCD without MCU controller, you can use the Python demo.

Install the dependent library files:

```bash
sudo apt update
sudo apt install python3-pil python3-pip
sudo pip3 install adafruit-circuitpython-ssd1306
```

Test demo:

```bash
cd U6143_ssd1306/python 
sudo python3 ssd1306_stats.py
```
