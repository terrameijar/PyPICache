# Start `pypiserver` at boot

To ensure that `pypiserver` software starts up automatically at boot, create a new Linux service and use `systemd` to manage it.

## Create a start up script

Create a startup script that the service will manage, call it `start-pypi-server.sh`. Add the following content to it:

```bash
#! /bin/bash
/home/pi/.local/bin/pypi-server -p 8080 /home/pi/minirepo/ &
```

## Add script to path

Copy the script to `/usr/bin` and make it executable:

```shell
sudo cp start-pypi-server.sh /usr/bin/start-pypi-server.sh
sudo chmod +x /usr/bin/start-pypi-server.sh
```

## Create a unit file to define a systemd service. 

Create a unit file and name it `pypiserver.service`:


```shell
GNU nano 3.2         /lib/systemd/system/pypiserver.service                  
 
[Unit]
Description=A minimal PyPI server for use with pip/easy_install.
 
[Service]
Type=forking
ExecStart=/bin/bash /usr/bin/start-pypi-server.sh
User=pi
 
[Install]
WantedBy=multi-user.target
```

This defines a basic service. The `ExecStart` directive specifies the command that will be run to start the service.

Copy the unit file to `/etc/systemd/system` and give it permissions:

```shell
sudo cp pypiserver.service /etc/systemd/system/pypiserver.service
sudo chmod 644 /etc/systemd/system/pypiserver.service
```

## Start and Enable the Service

Once you have created a unit file, you can test the service:

```shell
sudo systemctl start pypiserver
```

Check the status of the pypiserver service:

```shell
sudo systemctl status pypiserver
```

This will produce output similar to this:

```shell
$ sudo systemctl status pypiserver
● pypiserver.service - A minimal PyPI server for use with pip/easy_install.
   Loaded: loaded (/etc/systemd/system/pypiserver.service; enabled; vendor prese
   Active: active (running) since Fri 2020-02-07 19:17:05 CAT; 2h 19min ago
  Process: 420 ExecStart=/bin/bash /usr/bin/start-pypi-server.sh (code=exited, s
 Main PID: 441 (pypi-server)
    Tasks: 4 (limit: 4915)
   Memory: 408.0M
   CGroup: /system.slice/pypiserver.service
           └─441 /usr/bin/python /home/pi/.local/bin/pypi-server -p 8080 /home/p
 
Feb 07 19:17:05 raspberrypi systemd[1]: Starting A minimal PyPI server for use w
Feb 07 19:17:05 raspberrypi systemd[1]: Started A minimal PyPI server for use wi
```

To stop or restart the service:

```shell
sudo systemctl stop pypiserver
sudo systemctl restart pypiserver
```

Finally, use the enable command to ensure that the service starts whenever the system boots:

```shell
sudo systemctl enable pypiserver
```