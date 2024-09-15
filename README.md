# Redesigned Garbanzo

Diffuser sa musique vers sa carte son via Airplay depuis sa Raspberry Pi ? Let's go !

## Comment initialiser la Raspberry Pi ?

Ce tuto est grandement inspiré de <https://www.xda-developers.com/build-airplay-receiver-using-raspberry-pi/> avec mes propres modifications.

Burn une image Raspberry OS Lite via Raspberry Pi Manager <https://www.raspberrypi.com/software/>, puis installer `shairplay-sync`

```sh
sudo apt-get install shairport-sync # cette étape peut être longue
sudo systemctl start shairport-sync # démarrer le service
sudo systemctl enable shairport-sync # démarrer le service automatiquement à chaque boot
```

Trouver le périphérique nécessaire pour la carte son

```sh
# Lister les périphériques disponibles
aplay -l
```

La sortie peut ressembler à :

```txt
**** List of PLAYBACK Hardware Devices ****
card 0: Headphones [bcm2835 Headphones], device 0: bcm2835 Headphones [bcm2835 Headphones]
  Subdevices: 8/8
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
  Subdevice #7: subdevice #7
card 1: M2X2 [M-Track 2X2], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 2: vc4hdmi [vc4-hdmi], device 0: MAI PCM i2s-hifi-0 [MAI PCM i2s-hifi-0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

Dans ce cas là, je veux connecter à l'appareil `M2X2`.

Mettre ce nom du périphérique dans le le fichier de conf de shairport :

```sh
sudo vi /etc/shairport-sync.conf
```

Trouver la ligne contenant `output_device` et mettre le périphérique recherché en préfixant par `hw:`

```txt
output_device = "hw:<nom du périphérique>"
```

Relancer shairport

```sh
sudo systemctl restart shairport-sync
```

Monter le son

```sh
sudo amixer sset PCM,0 100%
```

Votre appareil est visible en airplay !
