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
> dde-launch-time: cleanup_acts='unlink '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: trap 'eval $cleanup_acts' EXIT
>> dde-launch-time: mktemp -u /tmp/XXXXXX
> dde-launch-time: eol_lock=/tmp/0iIyJA
> dde-launch-time: mkfifo /tmp/0iIyJA
> dde-launch-time: chown deepin /tmp/0iIyJA
> dde-launch-time: cleanup_acts='unlink '\''/tmp/0iIyJA'\''; unlink '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: echo '[Desktop Entry]
Name=End Of Launching Notification
Type=Application
Exec=/bin/bash -c '\''/bin/date +%%s >/tmp/0iIyJA'\''
'
> dde-launch-time: cleanup_acts='unlink '\''/etc/xdg/autostart/end-of-launching.desktop'\''; unlink '\''/tmp/0iIyJA'\''; unlink '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
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
```unch-time: echo irq
> dde-launch-time: echo 32768
> dde-launch-time: echo
> dde-launch-time: echo 1
> dde-launch-time: cleanup_acts='echo >/sys/kernel/debug/tracing/trace; unlink '\''/etc/xdg/autostart/end-of-launching.desktop'\''; unlink '\''/tmp/0iIyJA'\''
; unlink '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: cleanup_acts='echo 0 >/sys/kernel/debug/tracing/tracing_on; echo >/sys/kernel/debug/tracing/trace; unlink '\''/etc/xdg/autostart/end-of-lau
nching.desktop'\''; unlink '\''/tmp/0iIyJA'\''; unlink '\''/etc/lightdm/lightdm.conf.d/auto-login.conf'\'''
> dde-launch-time: echo Launching DDE...
Launching DDE...
>> dde-launch-time: date +%s
> dde-launch-time: start_time=1463125153
> dde-launch-time: systemctl start lightdm
> dde-launch-time: read end_time

Message from syslogd@dd-deeepin-vm at May 13 15:39:14 ...
 eanup, done. Exitting...
> dde-launch-time: echo 0
> dde-launch-time: popd
> dde-launch-time: cat /sys/kernel/debug/tracing/trace
> dde-launch-time: set +x
DDE launching time is: 1 seconds
```

* `AUTO_LOGIN_USER=deepin`: the user name which is used to launch DDE
* `DEBUG=1`: if DEBUG contains non-empty string, run dde-launch-time in tracing mode

If you want to figure out why DDE took so long to launch, the kernel
tracing log is saved in currently directory, named dde-launch-time.log.
