# Lab 01 — Linux System Resource Monitoring

## Objective

Practise commands for checking available CPU resources, memory and swap usage, filesystem capacity, system uptime, and running processes.

## Environment

- **Operating system:** Ubuntu running under WSL2
- **Shell:** Bash

## Commands Practised

| Command | What it does | Why it is useful |
|---|---|---|
| `nproc` | Reports the number of processing units available to the current process. | Helps identify the CPU capacity available to the environment. |
| `free -h` | Displays memory and swap usage in human-readable units. | Helps assess memory pressure and available memory. |
| `df -h /` | Reports disk-space usage for the filesystem mounted at `/`. | Helps identify storage capacity and filesystem usage. |
| `uptime` | Shows how long the system has been running, the number of logged-in users, and load averages. | Provides a quick view of system availability and recent load. |
| `ps -eo user,pid,ppid,%cpu,%mem,comm --sort=-%cpu \| head -n 10` | Lists selected process details, sorts by CPU usage in descending order, and displays the first 10 lines. | Helps inspect running processes, their owners, parent processes, and resource usage. |

## Key Observations

- `nproc` reported **2** processing units available to the environment.
- `free -h` reported **2.8 GiB** total memory, approximately **2.4 GiB** available, and **2.0 GiB** swap with none in use at the time of capture.
- `df -h /` showed the root filesystem with approximately **955 GiB** available and **1%** usage.
- `uptime` reported about **4 hours 56 minutes** of uptime and low load averages (**0.02, 0.02, 0.00**).
- The process listing displayed process owners, PIDs, parent PIDs, CPU and memory percentages, and command names.

These are point-in-time observations; resource usage and process IDs can change as the system runs.

## Practical Evidence

The screenshot below records the commands and their terminal output.

![Lab 01 — Linux system resource monitoring evidence](./evidence/lab_01-linux-system-resource-monitoring.png)

[View the original evidence file](./evidence/lab_01-linux-system-resource-monitoring.png)

## Key Takeaways

These commands provide a quick baseline of system resources and activity. They are useful for routine Linux administration and initial triage when investigating resource constraints or unusual process activity. A single process listing is only a snapshot and does not, by itself, establish malicious activity.

## Scope

This was an informational exercise in my own Ubuntu/WSL2 environment. No exploitation or security testing against external systems was performed.
