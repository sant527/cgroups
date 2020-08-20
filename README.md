# My configuration of cgroups: 

after experimenting with pdf-crop-margins large documents with large page sises (6" x 22")

10mb memory out of 16GB and (100)/1024 = 10% of cpu
```bash
best is

echo $(( 10 * 1024 * 1024 )) > /sys/fs/cgroup/memory/groupname/cpulimited_simha/memory.limit_in_bytes
echo 100 > /sys/fs/cgroup/cpu/groupname/cpulimited_simha/cpu.shares

ALSO freezing slightly

echo $(( 100 * 1024 * 1024 )) > /sys/fs/cgroup/memory/groupname/cpulimited_simha/memory.limit_in_bytes
echo 100 > /sys/fs/cgroup/cpu/groupname/cpulimited_simha/cpu.shares

THIS  ALSO freezing a bit

echo $(( 2 * 1024 * 1024 * 1024 )) > /sys/fs/cgroup/memory/groupname/cpulimited_simha/memory.limit_in_bytes
echo 100 > /sys/fs/cgroup/cpu/groupname/cpulimited_simha/cpu.shares

THE BELOW IS freezing the cpu (dont use)

echo $(( 4 * 1024 * 1024 * 1024 )) > /sys/fs/cgroup/memory/groupname/cpulimited_simha/memory.limit_in_bytes
echo 300 > /sys/fs/cgroup/cpu/groupname/cpulimited_simha/cpu.shares
```

Conclusion for long running processes give less memory. and less cpu else bursting happens and freezes

Here pdf-crop-margins is a long running process

`taskset -c 0 with pdf-crop-margins freezing the cpu.` 

So use only cgroups

# what is lower latency means

![alt text](./images/u9C9gdsnyK.png)

# CGROUPS understanding

https://engineering.indeedblog.com/blog/2019/12/unthrottled-fixing-cpu-limits-in-the-cloud/

![alt text](./images/2020-08-20_23-16.png)

![alt text](./images/2020-08-20_23-18.png)

![alt text](./images/rsqhOMudAn.png)

# Archlinux example

![alt text](./images/2020-08-20_23-26.png)

# cpu.shares

![alt text](./images/2020-08-20_23-30.png)


# Controlling CPU Throttling

## "noisy neighbor" problem

> "noisy neighbor" problem - processes that consume as much CPU as they can get, reducing everyone else's share.

## cpu.cfs_period_us

![alt text](./images/2020-08-20_23-35.png)

## cpu.cfs_quota_us

![alt text](./images/2020-08-20_23-41.png)

## cpu.stat

![alt text](./images/2020-08-20_23-44.png)

## cgroups question using differen intervals (small and big)

![alt text](./images/TgNgXiOt8m.png)

## Answer

>Every process gets a turn, soon. However there is more re-scheduling overhead: Every time the time runs out, and there are other processes ready to run, there is a re-schedule.

>Re-scheduling involves saving all registers on the stack, saving the stack-pointer into the task-control-block, switching task-control-block, disabling/enabling parts of the virtual-page-table, reloading the stack-pointer, and restoring the registers. It can also cause more cache misses. So in short things run slower.

>For long-running non-interactive tasks a longer scheduler period is better.


# cgroups: examples

https://01.org/linuxgraphics/gfx-docs/drm/scheduler/sched-bwc.html

## definition

![alt text](./images/2020-08-21_00-20.png)

## examples

![alt text](./images/2020-08-21_00-23.png)

![alt text](./images/2020-08-21_00-24.png)





# taskset : Read the CPU Affinity of a Running Process

## Read the CPU Affinity of a Running Process

For example, to check the CPU affinity of a process with PID 1141.
```bash
$ taskset -cp 1141
pid 1141's current affinity list: 0-31
```

in above example, pid 1141 can be executed on CPU core 0-31.

## taskset example to run a process in particular core

![alt text](./images/2020-08-21_00-06.png)


## taskset will not restrain other processess

![alt text](./images/2020-08-21_00-08.png)
