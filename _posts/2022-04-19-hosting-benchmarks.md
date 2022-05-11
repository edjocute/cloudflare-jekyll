---
title: "Some Benchmarks"
date: 2022-04-19T11:53:00+08:00
last_modified_at: 2022-05-08T03:00:00+08:00
categories:
  - Blog
tags:
  - Web
toc: true
toc_sticky: true
toc_label: "Servers"
toc_icon: "fa-solid fa-server"
---

Collection of benchmarks using [YASB][yasb]:
```
curl -sL yabs.sh | bash
```

---
## 1) Hetzner CPX11 (Ashburn, VA):


```bash
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-02-18                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Tue 19 Apr 2022 03:42:20 AM UTC

Basic System Information:
---------------------------------
Processor  : AMD EPYC Processor
CPU cores  : 2 @ 2445.404 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ❌ Disabled
RAM        : 1.9 GiB
Swap       : 1.9 GiB
Disk       : 38.9 GiB

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 120.25 MB/s  (30.0k) | 1.45 GB/s    (22.6k)
Write      | 120.57 MB/s  (30.1k) | 1.45 GB/s    (22.7k)
Total      | 240.82 MB/s  (60.2k) | 2.90 GB/s    (45.4k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 1.86 GB/s     (3.6k) | 1.73 GB/s     (1.6k)
Write      | 1.96 GB/s     (3.8k) | 1.85 GB/s     (1.8k)
Total      | 3.83 GB/s     (7.4k) | 3.58 GB/s     (3.5k)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 1.88 Gbits/sec  | 1.44 Gbits/sec
Online.net      | Paris, FR (10G)           | 1.04 Gbits/sec  | 1.81 Gbits/sec
WorldStream     | The Netherlands (10G)     | busy            | busy
WebHorizon      | Singapore (400M)          | 132 Mbits/sec   | 90.5 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 2.14 Gbits/sec  | 5.61 Gbits/sec
Velocity Online | Tallahassee, FL, US (10G) | 1.32 Gbits/sec  | 3.79 Gbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 1.04 Gbits/sec  | 1.63 Gbits/sec
Iveloz Telecom  | Sao Paulo, BR (2G)        | busy            | busy

iperf3 Network Speed Tests (IPv6):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 2.07 Gbits/sec  | 1.28 Gbits/sec
Online.net      | Paris, FR (10G)           | 1.71 Gbits/sec  | 2.18 Gbits/sec
WorldStream     | The Netherlands (10G)     | 1.64 Gbits/sec  | 1.49 Gbits/sec
WebHorizon      | Singapore (400M)          | 569 Mbits/sec   | 402 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 2.72 Gbits/sec  | 5.35 Gbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 1.82 Gbits/sec  | 1.89 Gbits/sec

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 1075
Multi Core      | 2114
Full Test       | https://browser.geekbench.com/v5/cpu/14406255
```

---
## 2) RackNerd (San Jose):

<div class="notice" markdown="1">
```white
LEB New Website Special - 3.5GB KVM

3x vCPU Core
45 GB SSD Cached RAID-10 Storage
3.5 GB RAM
7000GB Monthly Premium Bandwidth
1Gbps Public Network Port
Full Root Admin Access
1 Dedicated IPv4 Address
KVM / SolusVM Control Panel - Reboot, Reinstall, Manage rDNS, & much more
```
</div>


```bash
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-02-18                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Tue 19 Apr 2022 04:42:15 AM BST

Basic System Information:
---------------------------------
Processor  : Intel(R) Xeon(R) CPU E5-2680 v2 @ 2.80GHz
CPU cores  : 3 @ 2799.998 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ✔ Enabled
RAM        : 3.3 GiB
Swap       : 3.5 GiB
Disk       : 42.2 GiB

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 61.54 MB/s   (15.3k) | 710.71 MB/s  (11.1k)
Write      | 61.64 MB/s   (15.4k) | 714.46 MB/s  (11.1k)
Total      | 123.18 MB/s  (30.7k) | 1.42 GB/s    (22.2k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 977.92 MB/s   (1.9k) | 917.52 MB/s    (896)
Write      | 1.02 GB/s     (2.0k) | 978.63 MB/s    (955)
Total      | 2.00 GB/s     (3.9k) | 1.89 GB/s     (1.8k)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 370 Mbits/sec   | 317 Mbits/sec
Online.net      | Paris, FR (10G)           | 206 Mbits/sec   | 160 Mbits/sec
WorldStream     | The Netherlands (10G)     | 316 Mbits/sec   | 305 Mbits/sec
WebHorizon      | Singapore (400M)          | 107 Mbits/sec   | 367 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 414 Mbits/sec   | 272 Mbits/sec
Velocity Online | Tallahassee, FL, US (10G) | 592 Mbits/sec   | 203 Mbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 684 Mbits/sec   | 647 Mbits/sec
Iveloz Telecom  | Sao Paulo, BR (2G)        | busy            | busy

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 362
Multi Core      | 949
Full Test       | https://browser.geekbench.com/v5/cpu/14406263
```

---

## 3) NetCup

<div class="notice" markdown="1">
```white
RS Ostern S OST22

4 Dedicated CPUs
8 GB RAM
320 GB SSD
```
</div>

```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-02-18                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Thu 21 Apr 2022 04:11:55 PM CEST

Basic System Information:
---------------------------------
Processor  : AMD EPYC 7702P 64-Core Processor
CPU cores  : 4 @ 1996.250 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ❌ Disabled
RAM        : 7.8 GiB
Swap       : 0.0 KiB
Disk       : 314.9 GiB

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 46.44 MB/s   (11.6k) | 442.39 MB/s   (6.9k)
Write      | 46.49 MB/s   (11.6k) | 444.72 MB/s   (6.9k)
Total      | 92.94 MB/s   (23.2k) | 887.11 MB/s  (13.8k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 1.24 GB/s     (2.4k) | 1.68 GB/s     (1.6k)
Write      | 1.30 GB/s     (2.5k) | 1.79 GB/s     (1.7k)
Total      | 2.54 GB/s     (4.9k) | 3.47 GB/s     (3.3k)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 744 Mbits/sec   | 2.37 Gbits/sec
Online.net      | Paris, FR (10G)           | 634 Mbits/sec   | 2.37 Gbits/sec
WorldStream     | The Netherlands (10G)     | 793 Mbits/sec   | 2.38 Gbits/sec
WebHorizon      | Singapore (400M)          | 403 Mbits/sec   | 423 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 502 Mbits/sec   | 1.72 Gbits/sec
Velocity Online | Tallahassee, FL, US (10G) | 22.2 Mbits/sec  | 66.5 Mbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 401 Mbits/sec   | 1.01 Gbits/sec
Iveloz Telecom  | Sao Paulo, BR (2G)        | busy            | busy

iperf3 Network Speed Tests (IPv6):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 629 Mbits/sec   | 2.34 Gbits/sec
Online.net      | Paris, FR (10G)           | 762 Mbits/sec   | 2.31 Gbits/sec
WorldStream     | The Netherlands (10G)     | 569 Mbits/sec   | 2.34 Gbits/sec
WebHorizon      | Singapore (400M)          | 493 Mbits/sec   | 419 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 509 Mbits/sec   | 1.70 Gbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 156 Mbits/sec   | 1.17 Gbits/sec

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 1048
Multi Core      | 4031
Full Test       | https://browser.geekbench.com/v5/cpu/14458075
```

## 4) Oracle ARM

```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-05-06                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Sat May  7 19:02:14 UTC 2022

ARM compatibility is considered *experimental*

Basic System Information:
---------------------------------
Uptime     : 0 days, 2 hours, 24 minutes
Processor  : Neoverse-N1
CPU cores  : 4 @ ??? MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ❌ Disabled
RAM        : 23.4 GiB
Swap       : 0.0 KiB
Disk       : 45.1 GiB
Distro     : Ubuntu 20.04.4 LTS
Kernel     : 5.13.0-1027-oracle

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 6.39 MB/s     (1.5k) | 26.75 MB/s     (418)
Write      | 6.40 MB/s     (1.6k) | 27.55 MB/s     (430)
Total      | 12.80 MB/s    (3.2k) | 54.30 MB/s     (848)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 24.24 MB/s      (47) | 23.91 MB/s      (23)
Write      | 26.32 MB/s      (51) | 26.67 MB/s      (26)
Total      | 50.57 MB/s      (98) | 50.58 MB/s      (49)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 668 Mbits/sec   | 172 Mbits/sec
Online.net      | Paris, FR (10G)           | busy            | busy
Hybula          | The Netherlands (40G)     | 755 Mbits/sec   | 1.78 Gbits/sec
Clouvider       | NYC, NY, US (10G)         | 4.01 Gbits/sec  | 3.60 Gbits/sec
Velocity Online | Tallahassee, FL, US (10G) | 1.86 Gbits/sec  | 1.65 Gbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 839 Mbits/sec   | 776 Mbits/sec

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 841
Multi Core      | 3296
Full Test       | https://browser.geekbench.com/v5/cpu/14774048
```


[hetzner]: https://www.hetzner.com/cloud
[racknerd]: https://www.newcoupons.info/racknerd-cheap-kvm-vps-reseller-offers/
[yasb]: https://github.com/masonr/yet-another-bench-script