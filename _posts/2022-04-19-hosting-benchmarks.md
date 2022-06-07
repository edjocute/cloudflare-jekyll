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

$28.99 USD + Tax = $30.73 Annually
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

## 3) NetCup Root Server Ostern S

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

## 5) [Greencloud](https://greencloudvps.com) EPYCMO-2

<div class="notice" markdown="1">
```white
Double RAM + Bandwidth for Triennial payments
No refund/Money back on this plan

3072MB RAM
1024MB SWAP
35GB NVMe 4.0 RAID-10 Hard drive
2 cores @ EPYC Milan CPU
1 IPv4
/112 IPv6
5TB Bandwidth
10Gbps Port
Linux OS
Kansas City, MO Location
SolusVM Control Panel
	
$100 USD Triennially
```
</div>


```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-05-06                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Sat 04 Jun 2022 05:44:43 AM BST

Basic System Information:
---------------------------------
Uptime     : 1 days, 11 hours, 23 minutes
Processor  : AMD EPYC 7543P 32-Core Processor
CPU cores  : 2 @ 2794.748 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ✔ Enabled
RAM        : 2.9 GiB
Swap       : 1024.0 MiB
Disk       : 33.4 GiB
Distro     : Ubuntu 20.04 LTS
Kernel     : 5.4.0-28-generic

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 388.71 MB/s  (97.1k) | 4.59 GB/s    (71.8k)
Write      | 389.73 MB/s  (97.4k) | 4.62 GB/s    (72.2k)
Total      | 778.45 MB/s (194.6k) | 9.21 GB/s   (144.0k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 10.53 GB/s   (20.5k) | 12.22 GB/s   (11.9k)
Write      | 11.08 GB/s   (21.6k) | 13.04 GB/s   (12.7k)
Total      | 21.62 GB/s   (42.2k) | 25.26 GB/s   (24.6k)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 1.62 Gbits/sec  | 1.84 Gbits/sec
Online.net      | Paris, FR (10G)           | 2.21 Gbits/sec  | 1.82 Gbits/sec
Hybula          | The Netherlands (40G)     | 1.64 Gbits/sec  | 1.70 Gbits/sec
Clouvider       | NYC, NY, US (10G)         | 6.12 Gbits/sec  | 6.35 Gbits/sec
Velocity Online | Tallahassee, FL, US (10G) | busy            | busy
Clouvider       | Los Angeles, CA, US (10G) | 3.71 Gbits/sec  | 4.21 Gbits/sec

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 1158
Multi Core      | 2298
Full Test       | https://browser.geekbench.com/v5/cpu/15283838
```

```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-05-06                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Tue 07 Jun 2022 05:00:35 PM +08

Basic System Information:
---------------------------------
Uptime     : 0 days, 0 hours, 12 minutes
Processor  : AMD EPYC 7543P 32-Core Processor
CPU cores  : 2 @ 2794.748 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ✔ Enabled
RAM        : 5.8 GiB
Swap       : 1024.0 MiB
Disk       : 33.4 GiB
Distro     : Ubuntu 20.04 LTS
Kernel     : 5.4.0-28-generic

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 387.49 MB/s  (96.8k) | 5.05 GB/s    (78.9k)
Write      | 388.51 MB/s  (97.1k) | 5.07 GB/s    (79.3k)
Total      | 776.00 MB/s (194.0k) | 10.13 GB/s  (158.2k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 13.26 GB/s   (25.9k) | 13.35 GB/s   (13.0k)
Write      | 13.97 GB/s   (27.2k) | 14.24 GB/s   (13.9k)
Total      | 27.23 GB/s   (53.1k) | 27.59 GB/s   (26.9k)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | busy            | busy
Online.net      | Paris, FR (10G)           | 2.12 Gbits/sec  | 1.82 Gbits/sec
Hybula          | The Netherlands (40G)     | 1.59 Gbits/sec  | busy
Clouvider       | NYC, NY, US (10G)         | 4.67 Gbits/sec  | busy
Velocity Online | Tallahassee, FL, US (10G) | busy            | busy
Clouvider       | Los Angeles, CA, US (10G) | 4.09 Gbits/sec  | busy

iperf3 Network Speed Tests (IPv6):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 1.41 Gbits/sec  | busy
Online.net      | Paris, FR (10G)           | busy            | busy
Hybula          | The Netherlands (40G)     | busy            | busy
Clouvider       | NYC, NY, US (10G)         | 4.88 Gbits/sec  | busy
Clouvider       | Los Angeles, CA, US (10G) | 3.60 Gbits/sec  | busy

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 1172
Multi Core      | 2231
Full Test       | https://browser.geekbench.com/v5/cpu/15337955
```

## 6) [Server Factory](https://lowendspirit.com/discussion/3957/50-off-ryzen-epyc-vds-nl-alipay-btc-cc-pp-unmetered-incoming-traffic/) AMD EPYC™ XS VDS

<div class="notice" markdown="1">
```white
AMD EPYC™ XS

CPU: 0,5 Core / 1 Thread
RAM: 4 GB
STORAGE: 50 GB
UPLINK: 1 Gbit/s
UNMETERED INCOMING TRAFFIC
Outgoing Traffic: 4 TB per month
IPv4: 1
IPv6: /64 Subnet

49,00 € 24,50 € / annually
Promotional Code for 50% OFF (RECURRING): EPYCRYZENVPS
```
</div>

```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-05-06                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Mon 06 Jun 2022 10:37:49 AM CEST

Basic System Information:
---------------------------------
Uptime     : 17 days, 15 hours, 21 minutes
Processor  : AMD EPYC 7702P 64-Core Processor
CPU cores  : 1 @ 1999.999 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ✔ Enabled
RAM        : 3.8 GiB
Swap       : 0.0 KiB
Disk       : 49.1 GiB
Distro     : Ubuntu 20.04.4 LTS
Kernel     : 5.4.0-91-generic

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 20.17 MB/s    (5.0k) | 258.86 MB/s   (4.0k)
Write      | 20.17 MB/s    (5.0k) | 260.22 MB/s   (4.0k)
Total      | 40.35 MB/s   (10.0k) | 519.09 MB/s   (8.1k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 997.98 MB/s   (1.9k) | 1.36 GB/s     (1.3k)
Write      | 1.05 GB/s     (2.0k) | 1.45 GB/s     (1.4k)
Total      | 2.04 GB/s     (4.0k) | 2.81 GB/s     (2.7k)

iperf3 Network Speed Tests (IPv4):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 1.03 Gbits/sec  | 947 Mbits/sec
Online.net      | Paris, FR (10G)           | 1.03 Gbits/sec  | 820 Mbits/sec
Hybula          | The Netherlands (40G)     | 1.04 Gbits/sec  | 996 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 520 Mbits/sec   | 889 Mbits/sec
Velocity Online | Tallahassee, FL, US (10G) | busy            | busy
Clouvider       | Los Angeles, CA, US (10G) | 344 Mbits/sec   | 894 Mbits/sec

iperf3 Network Speed Tests (IPv6):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 1.03 Gbits/sec  | 982 Mbits/sec
Online.net      | Paris, FR (10G)           | 1.02 Gbits/sec  | 791 Mbits/sec
Hybula          | The Netherlands (40G)     | 1.04 Gbits/sec  | 985 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 528 Mbits/sec   | 915 Mbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 326 Mbits/sec   | 790 Mbits/sec

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 491
Multi Core      | 495
Full Test       | https://browser.geekbench.com/v5/cpu/15321522
```

## 7) [Server Factory](https://lowendspirit.com/discussion/3981/nl-ipv6-only-kvm-vps-6-year-1-core-1gb-ram-10gb-nvme-ssd#latest) KVM-IPV6-1 VPS

<div class="notice" markdown="1">
```white
CPU: 1 Core limited to 25% usage
RAM: 1 GB
STORAGE: 10 GB
UPLINK: 1 Gbit/s
TRAFFIC: 12 TB per year
IPv6 only with /64 Subnet

6 € annually
```
</div>

```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-05-06                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Mon 06 Jun 2022 04:47:31 PM +08

Basic System Information:
---------------------------------
Uptime     : 32 days, 23 hours, 10 minutes
Processor  : AMD EPYC 7702P 64-Core Processor
CPU cores  : 1 @ 1999.999 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ✔ Enabled
RAM        : 976.8 MiB
Swap       : 0.0 KiB
Disk       : 9.8 GiB
Distro     : Ubuntu 20.04.4 LTS
Kernel     : 5.4.0-109-generic

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 10.65 MB/s    (2.6k) | 130.07 MB/s   (2.0k)
Write      | 10.67 MB/s    (2.6k) | 130.76 MB/s   (2.0k)
Total      | 21.32 MB/s    (5.3k) | 260.83 MB/s   (4.0k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 442.85 MB/s    (864) | 473.53 MB/s    (462)
Write      | 466.38 MB/s    (910) | 505.07 MB/s    (493)
Total      | 909.23 MB/s   (1.7k) | 978.60 MB/s    (955)

iperf3 Network Speed Tests (IPv6):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 1.02 Gbits/sec  | 513 Mbits/sec
Online.net      | Paris, FR (10G)           | 1.01 Gbits/sec  | 434 Mbits/sec
Hybula          | The Netherlands (40G)     | 1.02 Gbits/sec  | 943 Mbits/sec
Clouvider       | NYC, NY, US (10G)         | 574 Mbits/sec   | 381 Mbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 382 Mbits/sec   | 524 Mbits/sec

Geekbench releases can only be downloaded over IPv4. FTP the Geekbench files and run manually.
Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 209
Multi Core      | 197
Full Test       | https://browser.geekbench.com/v5/cpu/15321839

---------------------------------
Test            | Value
                |
Single Core     | 206
Multi Core      | 210
Full Test       | https://browser.geekbench.com/v5/cpu/15322746
```


## 8) Scaleway Stardust AMS-1


```
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #
#              Yet-Another-Bench-Script              #
#                     v2022-05-06                    #
# https://github.com/masonr/yet-another-bench-script #
# ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## ## #

Mon Jun  6 17:34:12 UTC 2022

Basic System Information:
---------------------------------
Uptime     : 0 days, 4 hours, 34 minutes
Processor  : AMD EPYC 7281 16-Core Processor
CPU cores  : 1 @ 2096.058 MHz
AES-NI     : ✔ Enabled
VM-x/AMD-V : ✔ Enabled
RAM        : 973.3 MiB
Swap       : 512.0 MiB
Disk       : 8.9 GiB
Distro     : Ubuntu 20.04.4 LTS
Kernel     : 5.4.0-113-generic

fio Disk Speed Tests (Mixed R/W 50/50):
---------------------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 115.91 MB/s  (28.9k) | 888.55 MB/s  (13.8k)
Write      | 116.21 MB/s  (29.0k) | 893.22 MB/s  (13.9k)
Total      | 232.12 MB/s  (58.0k) | 1.78 GB/s    (27.8k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 1.07 GB/s     (2.0k) | 1.11 GB/s     (1.0k)
Write      | 1.12 GB/s     (2.2k) | 1.18 GB/s     (1.1k)
Total      | 2.20 GB/s     (4.3k) | 2.30 GB/s     (2.2k)

iperf3 Network Speed Tests (IPv6):
---------------------------------
Provider        | Location (Link)           | Send Speed      | Recv Speed
                |                           |                 |
Clouvider       | London, UK (10G)          | 359 Mbits/sec   | 313 Mbits/sec
Online.net      | Paris, FR (10G)           | 212 Mbits/sec   | 4.41 Gbits/sec
Hybula          | The Netherlands (40G)     | 207 Mbits/sec   | 4.32 Gbits/sec
Clouvider       | NYC, NY, US (10G)         | 212 Mbits/sec   | 2.01 Gbits/sec
Clouvider       | Los Angeles, CA, US (10G) | 239 Mbits/sec   | 1.13 Gbits/sec

Geekbench 5 Benchmark Test:
---------------------------------
Test            | Value
                |
Single Core     | 477
Multi Core      | 458
Full Test       | https://browser.geekbench.com/v5/cpu/15329166
```

[hetzner]: https://www.hetzner.com/cloud
[racknerd]: https://www.newcoupons.info/racknerd-cheap-kvm-vps-reseller-offers/
[yasb]: https://github.com/masonr/yet-another-bench-script