Sample init scripts and service configuration for jundcoind
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/jundcoind.service:    systemd service unit configuration
    contrib/init/jundcoind.openrc:     OpenRC compatible SysV style init script
    contrib/init/jundcoind.openrcconf: OpenRC conf.d file
    contrib/init/jundcoind.conf:       Upstart service configuration file
    contrib/init/jundcoind.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "jundcoin" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, jundcoind requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, jundcoind will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that jundcoind and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If jundcoind is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/jundcoin/jundcoin.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/jundcoin.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/jundcoind
Configuration file:  /etc/jundcoin/jundcoin.conf
Data directory:      /var/lib/jundcoind
PID file:            /var/run/jundcoind/jundcoind.pid (OpenRC and Upstart)
                     /var/lib/jundcoind/jundcoind.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the jundcoin user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
jundcoin user and group.  Access to jundcoin-cli and other jundcoind rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start jundcoind" and to enable for system startup run
"systemctl enable jundcoind"

4b) OpenRC

Rename jundcoind.openrc to jundcoind and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/jundcoind start" and configure it to run on startup with
"rc-update add jundcoind"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop jundcoind.conf in /etc/init.  Test by running "service jundcoind start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy jundcoind.init to /etc/init.d/jundcoind. Test by running "service jundcoind start".

Using this script, you can adjust the path and flags to the jundcoind program by
setting the JUNDCOIND and FLAGS environment variables in the file
/etc/sysconfig/jundcoind. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
