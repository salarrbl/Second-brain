### **What is an Init System?**
- An init system is the first process started by the Linux kernel (process ID 1 or PID 1) during boot. It initializes the user space, starts services, and sets up the environment for other processes

### **Runit**

**Runit** is a minimalistic, lightweight init system and service supervisor.
- ### Programs
-  `sv` - used for controlling services, getting status of services, and dependency checking.
- `chpst` - control of a process environment, including memory caps, limits on cores, data segments, environments, user/group privileges, and more.
- `runsv` - supervises a process, and optionally a log service for that process.
- `svlogd` - a simple but powerful logger, includes autorotation based on different methods (time, size, etc), post-processing, pattern matching, and socket (remote logging) options.
- `runsvchdir` - changes service levels (runlevels, see below)
- `runsvdir` - starts a supervision tree
- `runit-init` - PID 1, does almost nothing besides being the init
- ### Files
	-  `/etc/runit/1` - stage 1, system’s one-time initialization tasks
	- `/etc/runit/2` - stage 2, Normally runs `runsvdir`, should not return until the system is going to halt or reboot.
	- `/etc/runit/3` - stage 3, system’s shutdown tasks
	- `/etc/runit/ctrlaltdel` - Runit will execute this when receiving a `SIGINT` signal
	- `/etc/runit/runsvdir/*` - Runlevels
	- `/etc/runit/sv/*` - directory containing subdirectories of available service files
	- `/run/runit/service` - always symlinked to active runlevel, `sv` will search for running service here

However, since `runit` itself depends on `runit-rc`, there will be several extra rc files installed, most contained in `/etc/rc` and `/usr/lib/rc`
## **Managing Services with Runit**
**Add a Service** Each service is a directory under `/etc/sv/` with specific files:

- **`run`**: A script that defines how to start the service.
- **`finish`** (optional): A script executed when the service stops.
- **`log`** (optional): A directory for logging, containing its own `run` script.
- Example create Apache
```bash
sudo mkdir -p /etc/sv/httpd
sudo vim /etc/sv/httpd/run
```
- example run `script`
```bash
#!/bin/sh
	exec /usr/bin/httpd   -DFOREGROUND
```
-  add `x` perm for run files 
- ***Enable** a service
```bash
sudo ln -s /etc/sv/httpd /run/runit/service/httpd
```
- **Disable** a service
```bash
sudo rm -rf /run/runit/service/httpd
```
- status
```shell
sudo sv status httpd
```

### **Systemd**

**Systemd** is a feature-rich and modern init system that has become the standard on most major Linux distributions.