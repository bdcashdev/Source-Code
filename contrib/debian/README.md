
Debian
====================
This directory contains files used to package jundcoind/jundcoin-qt
for Debian-based Linux systems. If you compile jundcoind/jundcoin-qt yourself, there are some useful files here.

## jundcoin: URI support ##


jundcoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install jundcoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your jundcoinqt binary to `/usr/bin`
and the `../../share/pixmaps/jundcoin128.png` to `/usr/share/pixmaps`

jundcoin-qt.protocol (KDE)

