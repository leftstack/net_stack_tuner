<div align="center">

  # NetStack Tuner

</div>

NetStack Tuner is a small Windows GUI for inspecting and changing selected TCP template, global TCP stack, and network-adapter settings. It is aimed at users who need a controlled way to tune Windows networking behavior without manually assembling `netsh`/PowerShell/registry commands.

![screenshot](https://raw.githubusercontent.com/leftstack/net_stack_tuner/main/netstack102.png)

## Highlights

- **Native desktop UI** built with Rust & Slint.
- **TCP template controls** for `Internet`, `Datacenter`, `Compat`, `InternetCustom`, and `DatacenterCustom`.
- **Congestion provider selection** including `Default`, `NewReno`, `CTCP`, `DCTCP`, `LEDBAT`, `CUBIC`, and `BBR2` where supported.
- **Global TCP controls** for RSS, RSC, ECN, timestamps, auto-tuning, initial RTO, non-SACK RTT resiliency, max SYN retransmissions, TCP Fast Open, HyStart, PRR, and pacing profile.
- **Dynamic port range editors** for TCP, UDP, and TCP auto-reuse port ranges.
- **Registry controls** for `TcpTimedWaitDelay` and Path MTU Discovery (PMTUD).
- **Per-adapter controls** for IPv4/IPv6 MTU, Large Send Offload, TCP checksum offload, Receive-Side Scaling, and Receive Segment Coalescing when supported by the adapter and driver.
- **Path MTU test** for finding the largest IPv4 packet that reaches a hostname or IPv4 address without fragmentation; the test does not change adapter settings.
- **Batch apply workflow** with read-back verification for supported changes.

## Supported Areas

NetStack Tuner can inspect and modify:

- TCP supplemental/template settings.
- TCP global settings.
- TCP and UDP dynamic port ranges, plus the TCP auto-reuse port range, as paired start/count settings.
- `TcpTimedWaitDelay` and `EnablePMTUDiscovery` under `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters`.
- Per-adapter IPv4/IPv6 MTU and supported offload settings.

BBR2 is guarded behind Windows 11 22H2 / build `22621` or newer because older Windows builds do not expose usable BBR2 support.

## Safety Notes

- Network stack changes can affect every application on the machine; adapter changes can affect connectivity and performance for the selected interface.
- Some settings may require a reboot before Windows fully applies them. Changing PMTUD requires a restart.
- Record baseline values before experimenting so they can be restored.
- Avoid changing dynamic port ranges or adapter MTU/offload settings on production systems without understanding the workload, network path, and driver support.
- The Path MTU test currently supports IPv4 destinations only. Inconclusive ICMP responses, including timeouts, do not produce an MTU result.

## Disclaimer

NetStack Tuner is a configuration tool, not an automatic optimizer. The best TCP settings depend on Windows build, network adapter behavior, workload, path latency, and loss characteristics. Test changes deliberately & restore known-good values if behavior regresses.

---

*Note: This repository contains binary releases only.*
