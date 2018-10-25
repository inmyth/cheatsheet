## Systemd


### Typical
- create a file `sudo vi /lib/systemd/system/xxx.service`
```
[Unit]
Description=XXX App
After=network.target

[Service]
StartLimitIntervalSec=0
Type=simple
Restart=on-abnormal
RestartSec=10
ExecStart=/usr/bin/java -jar /srv/martiply/vertx-martiply-api-assembly-0.1.5.jar  /srv/martiply/log/

[Install]
WantedBy=multi-user.target

```

- Run/Stop
```
sudo systemctl restart xxx.service
sudo systemctl stop xxx.service

```
- reload change
```
sudo systemctl daemon-reload
```

### Enviroment variables
Systemd uses its own environment variables which are independent from system environment variables. In short any variables in ~/.bashrc, profile etc will not be loaded.

To set systemd environment variables,
- create `/etc/systemd/system/xxx.service.d/override.conf`
- add in it
```
[Service]
Environment="SECRET=pGNqduRFkB4K9C2vijOmUDa2kPtUhArN"
Environment="ANOTHER_SECRET=JP8YLOc2bsNlrGuD6LVTq7L36obpjzxd"
```
- ref https://serverfault.com/questions/413397/how-to-set-environment-variable-in-systemd-service
