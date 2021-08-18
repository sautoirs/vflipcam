# vflipcam

I own a ASUS K52F with a Chicony webcam (04f2:b1e5) mounted upside down. This repository is used as a workaround to
vflip the video stream on linux ubuntu 20.0.

## Install

We need ffmpeg and akvcam.

### ffmpeg

```
sudo apt install ffmpeg
```

### akvcam

It should be compiled from source using dkms (see the [wiki](
https://github.com/webcamoid/akvcam/wiki/Build-and-install)).

Then the config file should be created in `/etc/akvcam/config.ini`.

```bash
sudo mkdir -p /etc/akvcam
sudo cp ./config.ini /etc/akvcam/config.ini
# Reload the driver for the changes to take effect
sudo rmmod akvcam; sudo modprobe akvcam
```

You can also start the module akvcam at startup by using:

```bash
sudo sh -c "echo akvcam >> /etc/modules-load.d/akvcam.conf"
```

## Run

Use the following command to fill the stream video3 with a vertically flipped version of video0.

```bash
ffmpeg -f v4l2 -i /dev/video0 -vf vflip -f v4l2 -vsync vfr /dev/video4
```

I usually set a bash alias for this command in my `~/.bash_aliases`

```bash
alias vflipcam='ffmpeg -f v4l2 -i /dev/video0 -vf vflip -f v4l2 -vsync vfr /dev/video4'
```

## Test

You can check if the video is flipped using:

```bash
ffplay /dev/video3
```

