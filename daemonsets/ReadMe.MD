#### DaemonSet Update Strategy
1. RollingUpdate: gradually scale down the old daemonsets and creates the new ones.
2. OnDelete: new daemonsets are recreated when the existing daemonsets are manually deleted.