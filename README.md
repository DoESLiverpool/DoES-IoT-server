# mqtt-in-a-box
Trivial install of Mosquitto onto a Raspberry Pi to set up a simple local MQTT broker

## Installation

### Pi

1. Install [Ansible](https://www.ansible.com/get-started) on your computer
1. Install the latest [Raspbian lite image](https://www.raspberrypi.org/downloads/raspbian/) onto a micro-SD card
1. Create a file called `ssh` on the `/boot` partition of the SD card.  The contents don't matter, it's just to enable the SSH server.  E.g. on Ubuntu `touch /media/myusername/boot/ssh`
1. Boot the Raspberry Pi with the micro-SD card, while plugged into a network via Ethernet
1. Find out the IP address of the Raspberry Pi
1. Copy your SSH credentials onto the Pi
  ```ssh-copy-id pi@<ip-address-of-the-pi>```
1. Edit the ```hosts``` file so ansible knows which computer to configure.  Change the IP address in it to match the one you just found out.
1. Check you can run commands on the Pi using Ansible
   ```ansible mqttiab -i hosts -a "hostname" -u pi```
1. Update the Pi
   ```ansible-playbook mqttiab.yml -i hosts```

## Usage

Once the install is complete, you'll have a local MQTT broker available at [http://mqtt.local:1883](http://mqtt.local:1883)

You can test that it works with `mosquitto_sub` and `mosquitto_pub`.  In one terminal run:

```
  mosquitto_sub -h mqtt.local -t test/+
```

Then in a separate terminal publish messages to the "test" topic:

```
  mosquitto_pub -h mqtt.local -t test/testing -m "Hello world"
```

If all is working well you should see "Hello world" printed in the `mosquitto_sub` terminal window.
