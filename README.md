# FactorioServerInstall
A simple bootstrap for Factorio servers on linux

# Preparation

Create a user named factorio with its home directory at /opt/games/factorio
`sudo useradd -U -r -s /usr/sbin/nologin -m -d /opt/games/factorio factorio`

Login as that user:
`sudo -u factorio -s`

Change to our homedirectory
`cd ~`

Download Factorio headless server
`wget https://factorio.com/get-download/1.1.57/headless/linux64`

rename file
`mv linux64 factorio-headless.tar.xz`

extract file
`tar --xz -xf factorio-headless.tar.xz`

move extracted contents into current directory and remove empty directory
`mv -R ./factorio/* ./*`
`rm -R ./factorio/`
