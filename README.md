# FactorioServerInstall
A simple bootstrap for Factorio servers on linux

This was feature in our live stream on 18/04/2022 : https://www.youtube.com/watch?v=6aXq8zwLV_g

## Preparation

- Create a user named factorio with its home directory at /opt/games/factorio
`sudo useradd -U -r -s /usr/sbin/nologin -m -d /opt/games/factorio factorio`

- Login as that user:
`sudo -u factorio -s`

- Change to our homedirectory
`cd ~`

- Download Factorio headless server
`wget https://factorio.com/get-download/1.1.57/headless/linux64`

- rename file
`mv linux64 factorio-headless.tar.xz`

- extract file
`tar --xz -xf factorio-headless.tar.xz`

- move extracted contents into current directory and remove empty directory plus the normal examples (we will recover them in the next step)
`mv -Rf ./factorio/* ./*`
`rm -Rf ./factorio/`
`rm -f ./data/*.example.json`

- clone this repository
`git clone https://github.com/TheNoviceProspect/FactorioServerInstall.git ./repo/`

- move contents to correct locations
`mv -Rf ./repo/* ./*`

## Generate Map and install service

I would recommend to edit the following files so you can change values prefixed with ***tnp-*** to your own liking, as well as map and difficulty settings!
Once the map has been generated you cannot change map settings, you would have to delete your world and start over, so be sure before the next step!!!

- `./data/map-gen-settings.json`
- `./data/map-settings.json`
- `./data/server-settings.json`
- `./factorio.service`
- `./screen-connect.sh`

Let's move our *screen-connect* script into a global executable path (if we can, this should work I reckon)

`mv ./screen-connect.sh /usr/local/sbin/screen-connect.sh`

Now generate a save game (including a map) to load into the server

`./create-server.sh`

Now we need to leave the context of the *factorio* user

`exit`

Let's copy our service and tell systemd about it

`sudo mv /opt/games/factorio/factorio.service /etc/systemd/system/`
`sudo systemctl daemon-reload`

Now we can enable and start the service

`sudo systemctl enable factorio.service`
`sudo systemctl start factorio.service`

## Et Voilá

You've got a running Factorio server, now what?

To connect to can either call our little script, like so: `screen-connect factorio`
(Please note: This script is VERY dumb, no minecraft support)
Or you could also simply call this yourself: `sudo -u factorio screen -R *prefix*factorio`
(Obviously change *prefix* to whatever suits you or leave it completely away, check my comment above, in regards to checking some files....)