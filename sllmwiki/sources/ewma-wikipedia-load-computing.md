# Load (computing) - Wikipedia

From Wikipedia, the free encyclopedia

| [[image: icon](<https://thumb.wikimedia.org/wikipedia/en/thumb/9/99/Question_book-new.svg/60px-Question_book-new.svg.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Question_book-new.svg>) | This article **needs [more citations](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability>)**. Please help [improve this article](<https://en.wikipedia.org/wiki/Special:EditPage/Load_(computing)>) by [adding citations to reliable sources](<https://en.wikipedia.org/wiki/Help:Referencing_for_beginners>). Unsourced material may be challenged and [removed](<https://en.wikipedia.org/wiki/Wikipedia:Verifiability#Burden_of_evidence>).\ *Find sources:* ["Load" computing](<https://www.google.com/search?as_eq=wikipedia&q=%22Load%22+computing>) – [news](<https://www.google.com/search?tbm=nws&q=%22Load%22+computing+-wikipedia&tbs=ar:1>) **·** [newspapers](<https://www.google.com/search?&q=%22Load%22+computing&tbs=bkt:s&tbm=bks>) **·** [books](<https://www.google.com/search?tbs=bks:1&q=%22Load%22+computing+-wikipedia>) **·** [scholar](<https://scholar.google.com/scholar?q=%22Load%22+computing>) **·** [JSTOR](<https://www.jstor.org/action/doBasicSearch?Query=%22Load%22+computing&acc=on&wc=on>) *(November 2010)**([Learn how and when to remove this message](<https://en.wikipedia.org/wiki/Help:Maintenance_template_removal>))* |
|---|---|

[[image: ](<https://thumb.wikimedia.org/wikipedia/commons/thumb/2/2d/Big-load.png/250px-Big-load.png?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail>)](<https://en.wikipedia.org/wiki/File:Big-load.png>)

[htop](<https://en.wikipedia.org/wiki/Htop>) displaying a significant computing load (top right: *Load average:*)

In [UNIX](<https://en.wikipedia.org/wiki/UNIX>) [computing](<https://en.wikipedia.org/wiki/Computing>), the system **load** is a measure of the amount of computational work that a computer system performs. The **load average** represents the average system load over a period of time. It conventionally appears in the form of three numbers which represent the system load during the last one-, five-, and fifteen-minute periods.

## Load

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=1>)\]

The Unix *load number* refers to the number of processes using or waiting for [CPU](<https://en.wikipedia.org/wiki/Central_processing_unit>); i.e., the number of processes in the *ready queue* or [run queue](<https://en.wikipedia.org/wiki/Run_queue>). An idle computer has a load number of 0 (the idle process is not counted). Each running process increments the load number by 1. Each process that terminates decrements it by 1. Most UNIX systems count only processes in the *running* (on CPU) or *runnable* (waiting for CPU) [states](<https://en.wikipedia.org/wiki/Process_state>) (state R).

In addition to processes in "R" state, [Linux](<https://en.wikipedia.org/wiki/Linux>) also includes processes in uninterruptible sleep states (usually waiting for [disk](<https://en.wikipedia.org/wiki/Hard_disk_drive>) activity; state "D"), which can lead to markedly different results if many processes remain blocked in [I/O](<https://en.wikipedia.org/wiki/Input/output>) due to a busy or stalled I/O system.\[1\] This, for example, includes processes blocking due to an [NFS](<https://en.wikipedia.org/wiki/Network_File_System>) server failure or slow mass storage [media](<https://en.wikipedia.org/wiki/Data_storage>) (e.g., [USB](<https://en.wikipedia.org/wiki/USB>) 1 storage devices). Such circumstances can result in an elevated load average which does not reflect an actual increase in CPU use. The idea behind its inclusion is that while disk wait is not the same as CPU-wait, it still reflects how long a user needs to wait.

On modern UNIX systems, the treatment of [threading](<https://en.wikipedia.org/wiki/Thread_(computing)>) with respect to load averages varies. Some systems treat threads as processes for the purposes of load average calculation: each thread waiting to run will add 1 to the load. However, other systems, especially systems implementing so-called [M:N threading](<https://en.wikipedia.org/wiki/Thread_(computing)#M:N_(hybrid_threading)>), use different strategies, such as counting the process exactly once for the purpose of load (regardless of the number of threads), or counting only threads currently exposed to the kernel by the user-thread scheduler, which may depend on the level of concurrency set on the process. Linux appears to count each thread separately as adding 1 to the load.\[2\]

There is no standard way to obtain the length of the run queue across different Unix-like systems,\[a\] but a commonly-available way is through [parsing](<https://en.wikipedia.org/wiki/Parsing>) the output of the [ps](<https://en.wikipedia.org/wiki/Ps_(Unix)>) command, specifically `ps -ax -o stat`, and counting the number of lines starting with "R" (corresponding to processesin the "R" state). If desired, one can also add uninterruptible "disk" [wait state](<https://en.wikipedia.org/wiki/Wait_state>), labelled "D" on Linux and FreeBSD, "U" on [macOS](<https://en.wikipedia.org/wiki/MacOS>). `-M` may be used to get per-thread information on Linux and macOS, but not FreeBSD, where the option is instead `-H`.\[4\] (The load reported will never be 0, as the `ps` process itself is counted. To obtain the true load, subtract one from the count.)

On Linux specifically, the [procfs](<https://en.wikipedia.org/wiki/Procfs>) file `/proc/stat` contains two lines `procs_running` and `procs_blocked`, corresponding to scheduling entities (processes/threads) in "R" and "D" states respectively. This can be used to read the current load instead of `ps`. As with before, the load reported includes the program currently reading the procfs file, so subtract one from the sum to obtain the true load.\[5\]

### Compared to CPU utilization

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=2>)\]

The comparative study of different load indices carried out by Ferrari et al. reported that CPU load information based upon the CPU queue length does much better in load balancing compared to CPU utilization. The reason CPU queue length did better is probably because when a host is heavily loaded its CPU utilization is likely to be close to 100%, and it is unable to reflect the exact load level of the utilization. In contrast, CPU queue lengths can directly reflect the amount of load on a CPU. As an example, two systems, one with three and the other with six processes in the queue, are both very likely to have utilizations close to 100% although they would obviously differ in terms of process wait times.\[6\]

## Load average

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=3>)\]

All Unix and Unix-like systems generate a dimensionless [metric](<https://en.wikipedia.org/wiki/Software_metric>) of three "load average" numbers in the [kernel](<https://en.wikipedia.org/wiki/Kernel_(operating_system)>). Users can easily query the current result from a [Unix shell](<https://en.wikipedia.org/wiki/Unix_shell>) by running the `uptime` command:

```
$ uptime
 14:34:03 up 10:43,  4 users,  load average: 0.06, 0.11, 0.09
```

The [`w`](<https://en.wikipedia.org/wiki/W_(Unix)>) and [`top`](<https://en.wikipedia.org/wiki/Top_(software)>) commands show the same three load average numbers, as do a range of [graphical user interface](<https://en.wikipedia.org/wiki/Graphical_user_interface>) utilities. The underlying interface is `getloadavg()`, a C function present on most UNIX systems since 4.3BSD-Reno of 1990 (but is not part of [POSIX](<https://en.wikipedia.org/wiki/POSIX>)).\[7\] On Linux specifically, one can also read `/proc/loadavg` for this information. This file also provides instantaneous information on the number of processes in "R" state, the total number of processes, and the [process ID](<https://en.wikipedia.org/wiki/Process_ID>) of the most-recently-created process.\[8\]

Systems calculate the load *average* as the [exponentially damped/weighted moving average](<https://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average>) of the load *number*. The three values of load average refer to the past one, five, and fifteen minutes of system operation.\[9\]

Mathematically speaking, all three values always average all the system load since the system started up. They all decay exponentially, but they decay at different *speeds*: they decay exponentially by *e* after 1, 5, and 15 minutes respectively. Hence, the 1-minute load average consists of 63% (more precisely: 1 − 1/*e*) of the load from the last minute and 37% (1/*e*) of the average load since start up, excluding the last minute. For the 5- and 15-minute load averages, the same 63%/37% ratio is computed over 5 minutes and 15 minutes, respectively. Therefore, it is not technically accurate that the 1-minute load average only includes the last 60 seconds of activity, as it includes 37% of the activity from the past, but it is correct to state that it includes *mostly* the last minute.

### Interpretation

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=4>)\]

For single-CPU systems that are [CPU bound](<https://en.wikipedia.org/wiki/CPU_bound>), one can think of load average as a measure of system utilization during the respective time period. For systems with multiple CPUs, one must divide the load by the number of processors in order to get a comparable measure.

For example, one can interpret a load average of "1.73 0.60 7.98" on a single-CPU system as:

- During the last minute, the system was overloaded by 73% on average (1.73 runnable processes, so that 0.73 processes had to wait for a turn for a single-CPU system on average).
- During the last 5 minutes, the CPU was idling 40% of the time on average.
- During the last 15 minutes, the system was overloaded 698% on average (7.98 runnable processes, so that 6.98 processes had to wait for a turn for a single-CPU system on average).

This implies that this system could have handled all the work scheduled for the last minute if it were 1.73 times as fast.

In a system with four CPUs, a load average of 3.73 would indicate that there were, on average, 3.73 processes ready to run. Because this is fewer than 4, we know each one could be scheduled into a CPU and that no overloading is present.

## Reckoning CPU load

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=5>)\]

### Linux as an example

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=6>)\]

On Linux systems, the load-average is not calculated on each clock tick, but driven by a variable value that is based on the `HZ` frequency setting and tested on each clock tick. This setting defines the kernel clock tick rate in [hertz](<https://en.wikipedia.org/wiki/Hertz>) (times per second), and it defaults to 100 for 10 [ms](<https://en.wikipedia.org/wiki/Millisecond>) ticks. Kernel activities use this number of ticks to time themselves. Specifically, the `calc_load()` function (in loadavg.h, formerly sched.h), which calculates the load average, nominally runs every `LOAD_FREQ (5*HZ+1)` ticks, i.e. a tiny bit over 5 s.

```
extern unsigned long avenrun[];		/* Load averages */
extern void get_avenrun(unsigned long *loads, unsigned long offset, int shift);

#define FSHIFT		11		/* nr of bits of precision */
#define FIXED_1		(1<<FSHIFT)	/* 1.0 as fixed-point */
#define LOAD_FREQ	(5*HZ+1)	/* 5 sec intervals */
#define EXP_1		1884		/* 1/exp(5sec/1min) as fixed-point */
#define EXP_5		2014		/* 1/exp(5sec/5min) */
#define EXP_15		2037		/* 1/exp(5sec/15min) */

/* a1 = a0 * e + a * (1 - e) */
static inline unsigned long
calc_load(unsigned long load, unsigned long exp, unsigned long active)
{
	unsigned long newload;

	newload = load * exp + active * (FIXED_1 - exp);
	if (active >= load)
		newload += FIXED_1-1;

	return newload / FIXED_1;
}

extern unsigned long calc_load_n(unsigned long load, unsigned long exp,
				 unsigned long active, unsigned int n);

#define LOAD_INT(x) ((x) >> FSHIFT)
#define LOAD_FRAC(x) LOAD_INT(((x) & (FIXED_1-1)) * 100)
```

The avenrun array contains 1-minute, 5-minute and 15-minute average. The `calc_load()` function provides the correct update of the load-average for the default update rate of `LOAD_FREQ (5*HZ+1)`.\[10\] It is used like such in loadavg.c (formerly sched.c):\[11\]

```
void calc_global_load(void)
{
	unsigned long sample_window;
	long active, delta;

	sample_window = READ_ONCE(calc_load_update);
	if (time_before(jiffies, sample_window + 10))
		return;

	/*
	 * Fold the 'old' NO_HZ-delta to include all NO_HZ CPUs.
	 */
	delta = calc_load_nohz_read();
	if (delta)
		atomic_long_add(delta, &calc_load_tasks);

	active = atomic_long_read(&calc_load_tasks);
	active = active > 0 ? active * FIXED_1 : 0;

	avenrun[0] = calc_load(avenrun[0], EXP_1, active);
	avenrun[1] = calc_load(avenrun[1], EXP_5, active);
	avenrun[2] = calc_load(avenrun[2], EXP_15, active);

	WRITE_ONCE(calc_load_update, sample_window + LOAD_FREQ);

	/*
	 * In case we went to NO_HZ for multiple LOAD_FREQ intervals
	 * catch up in bulk.
	 */
	calc_global_nohz();
}
```

Note this handling of NO\_HZ CPUs. NO\_HZ is a mode meant to reduce the number of scheduling-clock interrupts on idle processors, which improves power efficiency and reduces clock jitter. This would cause processors to miss their update ticks, however. As a result, the counting of tasks is done using atomic operations. The function `calc_global_nohz` handles the calculation in case of needing to catch up to multiple ticks, making use of this function:

```
/* [1] application of the geometric series:
 *              n         1 - x^(n+1)
 *     S_n := \Sum x^i = -------------
 *             i=0          1 - x */
unsigned long
calc_load_n(unsigned long load, unsigned long exp,
	    unsigned long active, unsigned int n)
{
	return calc_load(load, fixed_power_int(exp, FSHIFT, n), active);
}
```

Here `fixed_power_int` (not included in article) raises exp to its nth power in fixed-point arithmetic.

### Sampling and precision

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=7>)\]

The "sampled" calculation of load averages is a somewhat common behavior; [FreeBSD](<https://en.wikipedia.org/wiki/FreeBSD>), too, only refreshes the value every five seconds. The interval is usually taken to not be exact so that they do not collect processes that are scheduled to fire at a certain moment. This is the reason for "+1" in the Linux code from above; FreeBSD instead uses a pseudorandom offset added to the interval.\[12\]

`loadavg.h` also mentions that the use of 11 fractional bits in the above fixed point calculation prevents reducing the interval (`LOAD_FREQ`) much lower. For example, a two-second interval would result in the EXP values becoming 1981, 2034 and 2043, nearly saturating the precision available (0–2047).\[10\]

Ripke Klaus has shown in 2011 that the "+1" modification alone is not sufficient to avoid [Moiré artifacts](<https://en.wikipedia.org/wiki/Moiré_pattern>) from regularly-scheduled processes. His experiments suggest 4.61 to be a better value: 0.61 is close to the [golden ratio](<https://en.wikipedia.org/wiki/Golden_ratio>), which helps spread out the sample-point among fractional seconds. At the same time, 4.61 is close to 60⁄13, so the property of 5 s being an integer fraction of 60 s is maintained.\[13\]\[14\] Ripke's change is common among [Android system](<https://en.wikipedia.org/wiki/Android_(operating_system)>) kernels, although the exact expression used (`4*HZ+61`) assumes an HZ of 100.\[15\] `60*HZ/13` would be more appropriate for varying values of HZ. The new values would be:\[14\]

```
#define LOAD_FREQ  (60*HZ/13)   /* 60/13 ~ 4.61 sec intervals */
#define EXP_1      1896         /* 1/exp(4.61sec/1min)  = 1/exp(1/13) as fixed-point */
#define EXP_5      2017         /* 1/exp(4.61sec/5min)  = 1/exp(1/13/5)  */
#define EXP_15     2038         /* 1/exp(4.61sec/15min) = 1/exp(1/13/15) */
```

### Calculation from userspace

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=8>)\]

As we've discussed before, most kernels use [fixed-point arithmetic](<https://en.wikipedia.org/wiki/Fixed-point_arithmetic>) to calculate the load average for efficiency and simplicity. This however limits the achievable update-rate and precision. Given that we have established how to get the instantaneous load from userspace, it is also possible to calculate the load average from userspace. The following code does so in Python, using a [refresh rate](<https://en.wikipedia.org/wiki/Refresh_rate>) of *φ* ≈ 1.618 seconds, on time-spans ranging from 10 seconds to 1 hour:

```
#!/usr/bin/env python3
import time
import os
from datetime import datetime
from math import exp, log
from dataclasses import dataclass

LOGSYSPERIODS = [log(x) for x in [60, 300, 900]]
REFRESH_RATE = ((5**0.5) + 1) / 2  # Ripke's golden ratio
PERIODS = [10, 30, 60, 120, 300, 900, 1800, 3600]
COUNT_DISKWAIT = True  # Whether to include disk wait in load calculation


@dataclass
class LoadEntry:
    avg: float
    exp: float

def initialize_loads(now: int | float, lavgs: dict[int, LoadEntry]):
    """Initialize load averages based on linear interpolation of system load averages and instant load count."""
    sys = getloadavg()
    slopes = ((sys[0] - now) / 60, (sys[1] - sys[0]) / 240, (sys[2] - sys[1]) / 600)
    for period in [10, 30, 60, 120, 300, 900, 1800, 3600]:
        exp_factor = exp(-REFRESH_RATE / period)
        if period < 60:
            est_avg = now + slopes[0] * (period - 60)
        elif period < 300:
            est_avg = sys[0] + slopes[1] * (period - 300)
        else:
            est_avg = sys[1] + slopes[2] * (period - 900)
        lavgs[period] = LoadEntry(avg=max(est_avg, 0), exp=exp_factor)

def update_loads(lavgs: dict[int, LoadEntry], current_load: int | float) -> None:
    for _, entry in lavgs.items():
        entry.avg = entry.avg * entry.exp + current_load * (1 - entry.exp)

if os.name == "posix":
    uname = os.uname()[0].lower()
    getloadavg = os.getloadavg

    if uname == "linux":
        def get_current_load() -> int:
            load = 0
            with open("/proc/stat", "r") as f:
                for line in f:
                    if line.startswith("procs_running "):
                        load += int(line.split()[1])  # Read procs_running
                        load -= 1  # Subtract one for ourself
                    elif line.startswith("procs_blocked ") and COUNT_DISKWAIT:
                        load += int(line.split()[1])  # Read procs_blocked
            return load

    else:
        PS_THREAD_OPTION = "-H" if os.uname()[0].lower().endswith("bsd") else "-M"
        PS_DISK_WAIT = "U" if os.uname()[0] == "Darwin" else "D"
        PS_STATES = ("R" + PS_DISK_WAIT) if COUNT_DISKWAIT else "R"

        def get_current_load() -> int:
            with os.popen(f"ps {PS_THREAD_OPTION}ax -o stat", "r") as f:
                states = map(f, lambda line: line.split()[-1])  # Get the last column. Required on macOS!
                return sum(1 if state[0] in PS_STATES else 0 for state in states) - 1

elif os.name == "nt":
    # It is possible to use Windows performance counters to get a queue length.
    # In fact, Microsoft recommends it as an additional metric for load in addition to CPU usage:
    # https://learn.microsoft.com/en-us/biztalk/technical-guides/using-the-performance-analysis-of-logs-pal-tool#processor-queue-length-analysis
    # Using it we can also obtain a load similar to Unix systems.
    from pyperfmon import pyperfmon

    pm = pyperfmon.pyperfmon()
    ncores = os.cpu_count()
    get_counter = lambda x: pm.getCounter(x)

    def get_current_load() -> float:
        return (
            # Threads waiting for CPU, not running ones
            get_counter(r"System\Processor Queue Length")
            # Approximate number of threads using CPU
            + get_counter(r"Processor\_Total\% Processor Time") * ncores
            # Threads waiting for disk I/O
            + get_counter(r"PhysicalDisk\_Total\Current Disk Queue Length")
            if COUNT_DISKWAIT
            else 0
        )

    def getloadavg() -> tuple[float, float, float]:
        # Dummy implementation for Windows
        load = get_current_load()
        return (load, load, load)

def main():
    lavgs: dict[int, LoadEntry] = {}
    current_load = get_current_load()
    initialize_loads(current_load, lavgs)
    heading = ["SYSTIME", " CURR"] + [str(x) for x in PERIODS]
    print("\t".join(heading))
    while True:
        # Always print before recalculating
        entries = [f"{datetime.now().strftime('%H:%M:%S')}  {current_load:.4f}"]
        entries += [f"{entry.avg:.4f}" for entry in lavgs.values()]
        print("\t".join(entries), flush=True)

        # Sleep and wait. This assumes that the time used to update a new line is
        # minuscule compared to the time slept. If this is not the case, a
        # sleepuntil()-type reckoning should be used.
        time.sleep(REFRESH_RATE)

        # Print
        current_load = get_current_load()
        update_loads(lavgs, current_load)

if __name__ == "__main__":
    main()
```

## Other system performance commands

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=9>)\]

Other commands for assessing system performance include:

- `uptime` –  the system reliability and load average
- `top` –  for an overall system view
  - `htop` –  interactive process viewer
- `btop` –  another overall system view tool
- `vmstat` –  vmstat reports information about runnable or blocked processes, memory, paging, block I/O, traps, and CPU.
- `dool` (formerly `dstat`),\[16\] `atop` –  helps correlate all existing resource data for processes, memory, paging, block I/O, traps, and CPU activity.
- `iftop` –  interactive network traffic viewer per interface
- `nethogs` –  interactive network traffic viewer per process
- `iotop` –  interactive I/O viewer\[17\]
- `iostat` –  for storage I/O statistics
- `netstat` –  for network statistics
- `mpstat` –  for CPU statistics
- `tload` –  load average graph for terminal
- `xload` –  load average graph for X

## See also

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=10>)\]

- [CPU usage](<https://en.wikipedia.org/wiki/CPU_usage>)

## Notes

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=11>)\]

1. ↑ The optional XSI part of POSIX does provide for `ps -axl`, which gives a multi-column output that can be parsed containing the state.\[3\] Unfortunately it is even less consistently implemented than `-o stat` and takes more work to parse.

\

## References

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=12>)\]

1. ↑ *["Linux Tech Support: What exactly is a load average?"](<https://linuxtechsupport.blogspot.com/2008/10/what-exactly-is-load-average.html>). 23 October 2008.*
2. ↑ See [http://serverfault.com/a/524818/27813](<http://serverfault.com/a/524818/27813>)
3. ↑ `ps` – Shell and Utilities Reference, [The Single UNIX Specification](<https://en.wikipedia.org/wiki/Single_Unix_Specification>), Version 5 from [The Open Group](<https://en.wikipedia.org/wiki/The_Open_Group>)
4. ↑ `ps(1)` – [Linux](<https://en.wikipedia.org/wiki/Linux>) User [Manual](<https://en.wikipedia.org/wiki/Man_page>) – User Commands from Manned.org`ps(1)` – [FreeBSD](<https://en.wikipedia.org/wiki/FreeBSD>) General Commands [Manual](<https://en.wikipedia.org/wiki/Man_page>)`ps(1)` – [Darwin](<https://en.wikipedia.org/wiki/Darwin_(operating_system)>) and [macOS](<https://en.wikipedia.org/wiki/MacOS>) General Commands [Manual](<https://en.wikipedia.org/wiki/Man_page>)
5. ↑ *["Miscellaneous kernel statistics in /proc/stat"](<https://www.kernel.org/doc/html/latest/filesystems/proc.html#miscellaneous-kernel-statistics-in-proc-stat>). Retrieved 4 October 2023.*
6. ↑ Ferrari, Domenico; and Zhou, Songnian; "[An Empirical Investigation of Load Indices For Load Balancing Applications](<http://www.eecs.berkeley.edu/Pubs/TechRpts/1987/CSD-87-353.pdf>)", Proceedings of Performance '87, the 12th International Symposium on Computer Performance Modeling, Measurement, and Evaluation, North Holland Publishers, Amsterdam, the Netherlands, 1988, pp. 515–528
7. ↑ `getloadavg(2)` – [FreeBSD](<https://en.wikipedia.org/wiki/FreeBSD>) System Calls [Manual](<https://en.wikipedia.org/wiki/Man_page>)
8. ↑ `proc_loadavg(5)` – [Linux](<https://en.wikipedia.org/wiki/Linux>) Programmer's [Manual](<https://en.wikipedia.org/wiki/Man_page>) – File Formats from Manned.org
9. ↑ *Walker, Ray (1 December 2006). ["Examining Load Average"](<https://www.linuxjournal.com/article/9001>). *Linux Journal*. Retrieved 13 March 2012.*
10. 1 2 *["Linux v6.17 :: include/linux/sched/loadavg.h"](<https://github.com/torvalds/linux/blob/v6.17/include/linux/sched/loadavg.h#L20>).*
11. ↑ *["Linux v6.17 :: kernel/sched/loadavg.c"](<https://github.com/torvalds/linux/blob/v6.17/kernel/sched/loadavg.c>).*
12. ↑ *["How is load average calculated on FreeBSD?"](<https://unix.stackexchange.com/a/342778>). *Unix & Linux Stack Exchange*.*
13. ↑ *Ripke, Klaus (2011). ["Linux-Kernel Archive: `LOAD_FREQ (4*HZ+61)` avoids `loadavg` Moire"](<https://lkml.iu.edu/hypermail/linux/kernel/1111.1/02446.html>). *lkml.iu.edu*.*
14. 1 2 *Ripke, Klaus (2011). ["Moiré patterns in linux load average"](<https://ripke.com/loadavg/moire>).* (graph demonstrating the pattern; later comment for 60\*HZ/13)
15. ↑ *["Patch kernel with the 4.61s load thing · Issue #2109 · AOSC-Dev/aosc-os-abbs"](<https://github.com/AOSC-Dev/aosc-os-abbs/issues/2109>). *GitHub*.*
16. ↑ *Baker, Scott (28 September 2022). ["dool - Python3 compatible clone of dstat"](<https://github.com/scottchiefbaker/dool>). *[GitHub](<https://en.wikipedia.org/wiki/GitHub>)*. Retrieved 22 November 2022. ...Dag Wieers ceased development of Dstat...*
17. ↑ *["Iotop(8) - Linux manual page"](<https://man7.org/linux/man-pages/man8/iotop.8.html>).*

## External links

\[[edit](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&action=edit&section=13>)\]

- *Brendan Gregg (8 August 2017). ["Linux Load Averages: Solving the Mystery"](<https://www.brendangregg.com/blog/2017-08-08/linux-load-averages.html>). Retrieved 22 January 2018.*
- *[Neil J. Gunther](<https://en.wikipedia.org/wiki/Neil_J._Gunther>). ["UNIX Load Average – Part 1: How It Works"](<http://www.teamquest.com/pdfs/whitepaper/ldavg1.pdf>) (PDF). TeamQuest. Retrieved 12 August 2009.*
- *Andre Lewis (31 July 2009). ["Understanding Linux CPU Load – when should you be worried?"](<https://blog.scoutapp.com/articles/2009/07/31/understanding-load-averages>). Retrieved 21 July 2011.* Explanation using an illustrated traffic analogy.
- *Ray Walker (1 December 2006). ["Examining Load Average"](<https://www.linuxjournal.com/article/9001>). Linux Journal. Retrieved 21 July 2011.*
- *Karsten Becker. ["Linux OSS load monitoring toolset"](<http://www.loadavg.com>). LoadAvg.*

Retrieved from "[https://en.wikipedia.org/w/index.php?title=Load\_(computing)&oldid=1368753991](<https://en.wikipedia.org/w/index.php?title=Load_(computing)&oldid=1368753991>)"

[Category](<https://en.wikipedia.org/wiki/Help:Category>):

- [Operating system technology](<https://en.wikipedia.org/wiki/Category:Operating_system_technology>)

Hidden categories:

- [Articles with short description](<https://en.wikipedia.org/wiki/Category:Articles_with_short_description>)
- [Short description is different from Wikidata](<https://en.wikipedia.org/wiki/Category:Short_description_is_different_from_Wikidata>)
- [Use dmy dates from September 2022](<https://en.wikipedia.org/wiki/Category:Use_dmy_dates_from_September_2022>)
- [Articles needing additional references from November 2010](<https://en.wikipedia.org/wiki/Category:Articles_needing_additional_references_from_November_2010>)
- [All articles needing additional references](<https://en.wikipedia.org/wiki/Category:All_articles_needing_additional_references>)
- [Articles with example C code](<https://en.wikipedia.org/wiki/Category:Articles_with_example_C_code>)


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://en.wikipedia.org/wiki/Load_(computing)>
- License: [CC-BY-SA-4.0](<https://creativecommons.org/licenses/by-sa/4.0/>)
