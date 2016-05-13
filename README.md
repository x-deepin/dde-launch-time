Overview
====
To optimize the launching time of DDE, this script provides the fundamental
information that you can use it to analyze why DDE behave in that way.

How to use this script
====
After your target system booted up, ssh into it (recommended way), run
```
$ sudo AUTO_LOGIN_USER=deepin DEBUG=1 ./dde-launch-time
> dde-launch-time: systemctl is-active lightdm
> dde-launch-time: systemctl stop lightdm
> dde-launch-time: mkdir -p /etc/lightdm/lightdm.conf.d
> dde-launch-time: echo '[SeatDefaults]
autologin-user=deepin
'
> dde-launch-time: cleanup_acts='unlink
> '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: trap 'eval $cleanup_acts' EXIT
>> dde-launch-time: mktemp -u /tmp/XXXXXX
> dde-launch-time: eol_lock=/tmp/tiiNbc
> dde-launch-time: mkfifo /tmp/tiiNbc
> dde-launch-time: chown deepin /tmp/tiiNbc
> dde-launch-time: cleanup_acts='unlink '\''/tmp/tiiNbc'\''; unlink
> '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: echo '[Desktop Entry]
Name=End Of Launching Notification
Type=Application
Exec=/bin/bash -c '\''/bin/date +%%s >/tmp/tiiNbc'\''
'
> dde-launch-time: cleanup_acts='unlink
> '\''/etc/xdg/autostart/end-of-launching.desktop'\''; unlink
> '\''/tmp/tiiNbc'\''; unlink '\''/etc/lightdm/lightdm.conf.
d/auto-login.conf'\'''
> dde-launch-time: pushd /sys/kernel/debug/tracing
> dde-launch-time: echo sched:sched_process_fork
> dde-launch-time: echo sched:sched_process_exec
> dde-launch-time: echo sched:sched_process_exit
> dde-launch-time: echo sched:sched_wakeup
> dde-launch-time: echo sched:sched_switch
> dde-launch-time: echo timer:timer_init
> dde-launch-time: echo timer:timer_start
> dde-launch-time: echo timer:timer_expire_entry
> dde-launch-time: echo timer:timer_expire_exit
> dde-launch-time: echo timer:hrtimer_start
> dde-launch-time: echo timer:hrtimer_expire_entry
> dde-launch-time: echo timer:hrtimer_expire_exit
> dde-launch-time: echo timer:itimer_expire
> dde-launch-time: echo workqueue
> dde-launch-time: echo power
> dde-launch-time: echo irq
> dde-launch-time: echo 32768
> dde-launch-time: echo
> dde-launch-time: echo 1
> dde-launch-time: cleanup_acts='echo >/sys/kernel/debug/tracing/trace; unlink
> '\''/etc/xdg/autostart/end-of-launching.desktop'\''; unlink
> '\''/tmp/tiiNbc'\''; unlink
> '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: cleanup_acts='echo 0 >/sys/kernel/debug/tracing/tracing_on;
> echo >/sys/kernel/debug/tracing/trace; unlink
> '\''/etc/xdg/autostart/end-of-launching.desktop'\''; unlink
> '\''/tmp/tiiNbc'\''; unlink
> '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
>> dde-launch-time: date +%s
> dde-launch-time: start_time=1463127340
> dde-launch-time: echo 1
> dde-launch-time: systemctl start lightdm
> dde-launch-time: read end_time

Message from syslogd@dd-deeepin-vm at May 13 16:15:41 ...
 eanup, done. Exitting...
> dde-launch-time: echo 0
> dde-launch-time: popd
> dde-launch-time: cat /sys/kernel/debug/tracing/trace
> dde-launch-time: set +x
DDE launching time is: 3 seconds
```

In the example above, last line shows that DDE spent 3 seconds to be launched.
The environment variables are explained below

* `AUTO_LOGIN_USER=deepin`: mandatory. the user name which is used to launch DDE
* `DEBUG=1`: optional. if DEBUG contains non-empty string, dde-launch-time runs
in tracing mode

If you wanna figure out why DDE took so long to launch, the kernel tracing log
is saved in current directory, named dde-launch-time.log.
