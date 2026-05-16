# TNN4113 Network Performance and Simulation — Complete Project Execution Guide

**Project:** Network Performance Analysis Using NS-3 Simulator
**Course:** TNN4113 Network Performance and Simulation
**Submission Date:** 21 May 2026
**Team Size:** 4 students
**Group:** Group 16

---

## Group Members

| Student | Name | Matric ID | Task |
|---|---|---|---|
| S1 | Brendan Chan Kah Le | 83403 | Task 1 — Point-to-Point Link Analysis |
| S2 | Xavier Liong Zhi Hao | 86079 | Task 2 — CSMA LAN Contention Study |
| S3 | Ng Clarence Chuan Hann | 84832 | Task 3 — TCP Congestion Control (Dumbbell) |
| S4 | Chong Ming Zin | 83489 | Task 4 — Synthesis & Report |

---

## ⚠️ Environment Specification (Your Actual Verified Setup)

> **Important for Ming Zin (S4):** The original report template says Ubuntu 22.04 / g++ 11.4 / Python 3.10. Your whole team is on **Ubuntu 24.04** with newer tools. You **must update Section 4.1 of the report** to match the table below — your terminal screenshots will expose the real versions and create a contradiction if left as 22.04.

> **ns-3 version:** Your group is using **ns-3.43** — this is what your lab already has and what all 4 members must install. Do not use any other version. Declare ns-3.43 in Section 4.1 of the report.

| Item | Version / Value |
|---|---|
| Operating System | **Ubuntu 24.04 LTS** (VirtualBox) |
| ns-3 release | **ns-3.43** |
| Compiler | **g++ 13.3.0** with C++17 |
| Build system | CMake **3.28.3** via `./ns3` wrapper |
| NetAnim version | bundled with whichever ns-3 version you download |
| Python | **3.12.3** |
| Qt / QMake | 5.15.13 |
| Plotting | gnuplot **6.0** / Python 3.12 + Matplotlib |
| Random-number seed | `RngSeedManager::SetSeed(1)` |
| Run number | RngRun = 1 (varied 1–5 for repeatability checks) |

> **Note on g++ 13 + ns-3:** Newer compilers are stricter. If `./build.py` crashes with C++ internal errors, see [Troubleshooting Section 13](#13-troubleshooting) for the fix.

---

## Table of Contents

1. [Team Roles & Responsibilities](#1-team-roles--responsibilities)
2. [Recommended Timeline (5 Days)](#2-recommended-timeline-5-days)
3. [Repository](#3-repository)
4. [Phase 0 — System Setup (Day 1, Everyone)](#4-phase-0--system-setup-day-1-everyone)
5. [Phase 1 — Install NS-3.43 (Day 1, Everyone)](#5-phase-1--install-ns-343-day-1-everyone)
6. [Phase 2 — Verify Environment (Day 1, Everyone)](#6-phase-2--verify-environment-day-1-everyone)
7. [Phase 3 — Install NetAnim (Day 1, Xavier mandatory)](#7-phase-3--install-netanim-day-1-xavier-mandatory)
8. [Phase 4a — Task 1: Point-to-Point (Brendan)](#8-phase-4a--task-1-point-to-point-brendan)
9. [Phase 4b — Task 2: CSMA LAN (Xavier)](#9-phase-4b--task-2-csma-lan-xavier)
10. [Phase 4c — Task 3: Dumbbell + TCP (Clarence)](#10-phase-4c--task-3-dumbbell--tcp-clarence)
11. [Phase 5 — Report Assembly (Ming Zin)](#11-phase-5--report-assembly-ming-zin)
12. [Phase 6 — Final Review & Submission](#12-phase-6--final-review--submission)
13. [Troubleshooting](#13-troubleshooting)
14. [Submission Checklist](#14-submission-checklist)

---

## 1. Team Roles & Responsibilities

| Student | Role | Simulation File | Key Deliverables |
|---|---|---|---|
| **Brendan** (S1) | Task 1 — Point-to-Point | `task1_p2p.cc` | `task1_results.csv`, Tables 5.1 & 5.2, Figure 5.2, Screenshot 5.1, Section 5 |
| **Xavier** (S2) | Task 2 — CSMA LAN | `task2_csma.cc` | `task2_csma.xml`, Tables 6.1 & 6.2, Figure 6.2, Screenshots 6.1 & 6.2, Section 6 |
| **Clarence** (S3) | Task 3 — Dumbbell TCP | `task3_dumbbell.cc` | `cwnd-newreno.dat`, `cwnd-cubic.dat`, `cwnd_comparison.png`, Table 7.1, Screenshot 7.1, Section 7 |
| **Ming Zin** (S4) | Task 4 — Synthesis & Report | None | Topology diagrams (Figs 5.1/6.1/7.1), Sections 1–4 & 8–12, final report merge, submission zip |

**Important:** Each student runs their own simulation **independently on their own laptop**. There is no shared simulation — you each have a different `.cc` file and generate different output files. Only at the end does Ming Zin collect everyone's outputs and assemble the final package.

---

## 2. Recommended Timeline (5 Days)

| Day | Date | Brendan (S1) | Xavier (S2) | Clarence (S3) | Ming Zin (S4) |
|---|---|---|---|---|---|
| **Day 1 — Sat 17 May** | Install NS-3, get `task1_p2p.cc` compiling | Install NS-3 + NetAnim, get `task2_csma.cc` compiling | Install NS-3, get `task3_dumbbell.cc` compiling | Set up GitHub repo structure, draft Sections 1–4, start topology diagrams |
| **Day 2 — Sun 18 May** | Run 20-combo sweep, generate CSV + Figure 5.2 | Run 1/3/5 sender scenarios, open NetAnim, take screenshots | Run dumbbell, generate .dat files, plot cWnd | Continue Sections 1–4, draw Figures 5.1/6.1/7.1 in draw.io |
| **Day 3 — Mon 19 May** | Fill Tables 5.1/5.2, write Section 5 analysis | Fill Tables 6.1/6.2, write Section 6 analysis | Fill Table 7.1, write Section 7 analysis | Draft Section 8 (Cross-Task Synthesis) |
| **Day 4 — Tue 20 May** | Push branch, open PR on GitHub | Push branch, open PR on GitHub | Push branch, open PR on GitHub | Merge all 3 PRs, pull main, assemble report, replace ALL `[INSERT]` placeholders, write Sections 9/10 |
| **Day 5 — Wed 21 May** | Final check of Section 5 in merged report | Final check of Section 6 in merged report | Final check of Section 7 in merged report | Export PDF, assemble zip, upload to eLeap before deadline |

---

## 3. Repository

```
https://github.com/chuanheng02/TNN4113-Network-Performance-Analysis.git
```

Use this folder structure inside the repo:

```
TNN4113-Network-Performance-Analysis/
├── README.md
├── src/
│   ├── task1_p2p.cc           ← Brendan
│   ├── task2_csma.cc          ← Xavier
│   └── task3_dumbbell.cc      ← Clarence
├── scripts/
│   ├── task1_sweep.sh         ← Brendan
│   ├── plot_task1.py          ← Brendan
│   ├── plot_task2.py          ← Xavier
│   └── plot_cwnd.py           ← Clarence
├── results/
│   ├── task1_results.csv      ← Brendan
│   ├── task2_csma.xml         ← Xavier (5-sender run)
│   ├── cwnd-newreno.dat       ← Clarence
│   ├── cwnd-cubic.dat         ← Clarence
│   ├── figure_5_2.png         ← Brendan
│   ├── figure_6_2.png         ← Xavier
│   └── cwnd_comparison.png    ← Clarence
├── screenshots/
│   ├── 5.1_task1_run.png      ← Brendan
│   ├── 6.1_netanim.png        ← Xavier
│   ├── 6.2_task2_terminal.png ← Xavier
│   └── 7.1_task3_run.png      ← Clarence
└── report/
    ├── TNN4113_Project_Report.docx  ← Ming Zin
    └── TNN4113_Project_Report.pdf   ← Ming Zin
```

---

## 4. Phase 0 — System Setup (Day 1, Everyone)

### 4.1 You are all on Ubuntu 24.04 via Oracle VirtualBox

No extra setup needed. Just open your Ubuntu VM and proceed from Section 4.2.

Make sure your VM has enough resources before starting the NS-3 build — it is CPU-heavy:

- **RAM:** at least 4 GB allocated to the VM (8 GB recommended)
- **CPU cores:** at least 2 (4 recommended)
- **Disk space:** at least 20 GB free inside the VM

To check in your VM:
```bash
free -h        # check available RAM
nproc          # check CPU cores available
df -h ~        # check free disk space
```

### 4.2 Update your system

```bash
sudo apt update && sudo apt upgrade -y
```

### 4.3 Install all required packages

> **This command is updated for Ubuntu 24.04.** Key changes from the old 22.04 command:
> - `qtchooser` removed — dropped from Ubuntu 24.04
> - `python3-full` added — required for Python environment management on 24.04
> - `python3-matplotlib` and `python3-numpy` installed via apt instead of pip — avoids the "externally managed environment" error on 24.04
> - `libgcrypt20-dev` used directly (not `libgcrypt-dev`)
> - `sqlite` removed — does not exist as a standalone package; `sqlite3` and `libsqlite3-dev` are sufficient

```bash
sudo apt install -y \
  build-essential cmake ninja-build pkg-config \
  g++ gdb gcc-multilib g++-multilib \
  python3 python3-dev python3-full python3-setuptools \
  git \
  qtbase5-dev qttools5-dev qttools5-dev-tools qt5-qmake \
  mpi-default-bin mpi-default-dev openmpi-bin openmpi-common openmpi-doc libopenmpi-dev \
  autoconf automake libxml2 libxml2-dev libgcrypt20-dev libgsl-dev \
  flex bison libfl-dev tcpdump sqlite3 libsqlite3-dev \
  libgtk-3-dev gnuplot wget tar bzip2 unzip \
  python3-matplotlib python3-numpy
```

### 4.4 Verify toolchain

Run each command and confirm the output matches:

```bash
g++ --version
# Expected: g++ (Ubuntu 13.3.0-...) 13.3.0

python3 --version
# Expected: Python 3.12.3

cmake --version
# Expected: cmake version 3.28.3

qmake --version
# Expected: QMake version 3.1 / Using Qt version 5.15.13

gnuplot --version
# Expected: gnuplot 6.0 patchlevel 0
```

---

## 5. Phase 1 — Install NS-3.43 (Day 1, Everyone)

### 5.1 Download the NS-3.43 tarball

```bash
cd ~
wget https://www.nsnam.org/releases/ns-allinone-3.43.tar.bz2
```

### 5.2 Extract and build

```bash
tar xjf ns-allinone-3.43.tar.bz2
cd ns-allinone-3.43
./build.py --enable-examples --enable-tests
```

This takes **15–40 minutes** depending on your VM resources. Do not interrupt it. Many compiler lines will scroll past — that is normal.

When it finishes you will see:

```
Build finished successfully.
```

Your NS-3 root directory is:

```
~/ns-allinone-3.43/ns-3.43/
```

All simulation commands from this point are run from inside that directory.

### 5.3 Alternative — Git clone (use only if tarball download fails)

```bash
cd ~
git clone https://gitlab.com/nsnam/ns-3-dev.git ns-3
cd ns-3
git checkout ns-3.43
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

---

## 6. Phase 2 — Verify Environment (Day 1, Everyone)

All 4 members must run these checks and confirm outputs match before doing anything else.

### 6.1 Check NS-3 version

```bash
cd ~/ns-allinone-3.43/ns-3.43
./ns3 show version
```

Expected:
```
ns-3.43
```

> Note: `--version` flag does not exist in ns-3.43. Use `./ns3 show version` instead. You can also just confirm by running `pwd` — if you are inside `ns-allinone-3.43/ns-3.43/` you are on the correct version.

### 6.2 Run hello-simulator

```bash
./ns3 run hello-simulator
```

Expected:
```
Hello Simulator
```

### 6.3 Run a full example

```bash
./ns3 run first
```

You should see TCP packet-exchange log lines. If this works, NS-3 is fully functional.

### 6.4 Share confirmation in group chat

Screenshot your terminal showing all three outputs above and share in the group chat (WhatsApp/Telegram). Once all 4 members confirm, proceed.

---

## 7. Phase 3 — Install NetAnim (Day 1, Xavier mandatory)

NetAnim is bundled inside the ns-allinone tarball. You only need to compile it.

### 7.1 Build NetAnim

```bash
# Adjust version number to match what you installed
cd ~/ns-allinone-3.43/netanim-3.113    # folder name varies by ns-3 version
make clean
qmake NetAnim.pro
make -j$(nproc)
```

> The NetAnim folder inside `ns-allinone-3.43/` is named `netanim-3.113`. Run `ls ~/ns-allinone-3.43/` to confirm after extraction.

A `NetAnim` executable will appear in that folder.

### 7.2 Launch NetAnim

Since you are all on Ubuntu in VirtualBox, the GUI works natively:

```bash
./NetAnim
```

A window will appear. If nothing happens, make sure your VM has **Guest Additions** installed (it enables proper display drivers). Install with:

```bash
sudo apt install -y virtualbox-guest-x11
reboot
```

### 7.3 Add NetAnim to your PATH (convenience)

```bash
# Adjust the path to match your actual netanim folder name
echo 'export PATH="$HOME/ns-allinone-3.43/netanim-3.113:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Now you can type `NetAnim` from anywhere.

---

## 8. Phase 4a — Task 1: Point-to-Point (Brendan)

**Goal:** Measure how channel delay and packet error rate degrade TCP throughput on a single P2P link.

**What you produce:** `task1_p2p.cc`, `task1_results.csv`, `figure_5_2.png`, Screenshot 5.1, Tables 5.1 & 5.2, Section 5.

### 8.1 Create the source file

```bash
cd ~/ns-allinone-3.43/ns-3.43/scratch
nano task1_p2p.cc
```

Paste the complete source code below:

```cpp
/*
 * TNN4113 Group 16 - Task 1
 * Point-to-Point Network: Link Characteristic & Reliability Analysis
 * Student: Brendan Chan Kah Le (83403)
 *
 * Topology:  Node 0  <-- 5 Mbps, variable delay, variable error -->  Node 1
 *            (BulkSendApplication / TCP source)         (PacketSink / TCP sink)
 *
 * Varied:    channel delay  in {2ms, 10ms, 30ms, 60ms, 100ms}
 *            error rate     in {0.00, 0.01, 0.03, 0.05}  (per-packet)
 *
 * Measured:  FlowMonitor throughput, lost packets, mean delay.
 */

#include "ns3/core-module.h"
#include "ns3/network-module.h"
#include "ns3/internet-module.h"
#include "ns3/point-to-point-module.h"
#include "ns3/applications-module.h"
#include "ns3/flow-monitor-module.h"

using namespace ns3;

NS_LOG_COMPONENT_DEFINE("Task1P2P");

int main(int argc, char *argv[])
{
    std::string delayStr = "10ms";
    double      errRate  = 0.00;
    uint32_t    simTime  = 30;

    CommandLine cmd;
    cmd.AddValue("delay",   "Channel propagation delay",         delayStr);
    cmd.AddValue("errRate", "Per-packet error rate at receiver",  errRate);
    cmd.AddValue("simTime", "Simulation time in seconds",         simTime);
    cmd.Parse(argc, argv);

    Config::SetDefault("ns3::TcpL4Protocol::SocketType",
                       StringValue("ns3::TcpNewReno"));

    NodeContainer nodes;
    nodes.Create(2);

    PointToPointHelper p2p;
    p2p.SetDeviceAttribute("DataRate", StringValue("5Mbps"));
    p2p.SetChannelAttribute("Delay",   StringValue(delayStr));

    NetDeviceContainer devs = p2p.Install(nodes);

    Ptr<RateErrorModel> em = CreateObject<RateErrorModel>();
    em->SetAttribute("ErrorRate", DoubleValue(errRate));
    em->SetAttribute("ErrorUnit",
                     EnumValue(RateErrorModel::ERROR_UNIT_PACKET));
    devs.Get(1)->SetAttribute("ReceiveErrorModel", PointerValue(em));

    InternetStackHelper stack;
    stack.Install(nodes);

    Ipv4AddressHelper addr;
    addr.SetBase("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer ifs = addr.Assign(devs);

    uint16_t port = 9;
    BulkSendHelper src("ns3::TcpSocketFactory",
                       InetSocketAddress(ifs.GetAddress(1), port));
    src.SetAttribute("MaxBytes", UintegerValue(0));
    ApplicationContainer srcApp = src.Install(nodes.Get(0));
    srcApp.Start(Seconds(1.0));
    srcApp.Stop(Seconds(simTime));

    PacketSinkHelper sink("ns3::TcpSocketFactory",
                          InetSocketAddress(Ipv4Address::GetAny(), port));
    ApplicationContainer sinkApp = sink.Install(nodes.Get(1));
    sinkApp.Start(Seconds(0.0));
    sinkApp.Stop(Seconds(simTime));

    FlowMonitorHelper fmh;
    Ptr<FlowMonitor> fm = fmh.InstallAll();

    Simulator::Stop(Seconds(simTime));
    Simulator::Run();

    fm->CheckForLostPackets();
    auto stats = fm->GetFlowStats();

    for (auto &kv : stats) {
        double thrMbps = kv.second.rxBytes * 8.0 / ((simTime - 1) * 1e6);
        double delayMs = 0.0;
        if (kv.second.rxPackets > 0) {
            delayMs = (kv.second.delaySum.GetSeconds() /
                       kv.second.rxPackets) * 1000.0;
        }
        std::cout << "Flow "         << kv.first
                  << "  Throughput=" << thrMbps   << " Mbps"
                  << "  MeanDelay="  << delayMs   << " ms"
                  << "  Lost="       << kv.second.lostPackets
                  << "  TxPackets="  << kv.second.txPackets
                  << "  RxPackets="  << kv.second.rxPackets
                  << std::endl;
    }

    Simulator::Destroy();
    return 0;
}
```

Save: `Ctrl+O` → Enter → `Ctrl+X`

### 8.2 Build and single test run

```bash
cd ~/ns-allinone-3.43/ns-3.43
./ns3 build
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.00"
```

Expected output:
```
Flow 1  Throughput=4.XX Mbps  MeanDelay=XX ms  Lost=0  TxPackets=N  RxPackets=N
```

### 8.3 Take Screenshot 5.1

Screenshot your terminal showing the `./ns3 build` success and the `Flow 1 Throughput=...` output line. Save as `5.1_task1_run.png`.

### 8.4 Run the 20-combination sweep

```bash
cd ~/ns-allinone-3.43/ns-3.43
nano task1_sweep.sh
```

Paste:

```bash
#!/bin/bash
# task1_sweep.sh — run all 20 (delay, error-rate) combinations
# TNN4113 Group 16 — Brendan Chan Kah Le (83403)

set -e
OUT="task1_results.csv"
echo "delay_ms,errRate,throughput_Mbps,meanDelay_ms,lost,tx,rx,pdr_pct" > "$OUT"

DELAYS=(2 10 30 60 100)
ERRS=(0.00 0.01 0.03 0.05)

for d in "${DELAYS[@]}"; do
  for e in "${ERRS[@]}"; do
    echo "Running delay=${d}ms errRate=$e ..."
    line=$(./ns3 run "scratch/task1_p2p --delay=${d}ms --errRate=$e" 2>&1 \
           | grep '^Flow' | head -1)

    thr=$(echo  "$line" | awk -F'Throughput=' '{print $2}' | awk '{print $1}')
    md=$(echo   "$line" | awk -F'MeanDelay='  '{print $2}' | awk '{print $1}')
    lost=$(echo "$line" | awk -F'Lost='       '{print $2}' | awk '{print $1}')
    tx=$(echo   "$line" | awk -F'TxPackets='  '{print $2}' | awk '{print $1}')
    rx=$(echo   "$line" | awk -F'RxPackets='  '{print $2}' | awk '{print $1}')

    if [ "$tx" -gt 0 ] 2>/dev/null; then
      pdr=$(awk -v r="$rx" -v t="$tx" 'BEGIN{printf "%.2f", 100*r/t}')
    else
      pdr="0.00"
    fi

    echo "$d,$e,$thr,$md,$lost,$tx,$rx,$pdr" >> "$OUT"
  done
done

echo ""
echo "Done. Results saved to $OUT"
column -s, -t "$OUT"
```

Run it:

```bash
chmod +x task1_sweep.sh
./task1_sweep.sh
```

Runtime: approximately 10–20 minutes for all 20 combinations.

### 8.5 Plot Figure 5.2

Create `plot_task1.py`:

```python
#!/usr/bin/env python3
"""plot_task1.py — Figure 5.2: TCP throughput vs channel delay and error rate.
TNN4113 Group 16 — Brendan Chan Kah Le (83403)
"""
import csv
import matplotlib.pyplot as plt
from collections import defaultdict

data = defaultdict(list)
with open('task1_results.csv') as f:
    for row in csv.DictReader(f):
        d   = float(row['delay_ms'])
        e   = float(row['errRate'])
        thr = float(row['throughput_Mbps'])
        data[e].append((d, thr))

plt.figure(figsize=(9, 6))
markers = ['o', 's', '^', 'D']
for (e, pts), m in zip(sorted(data.items()), markers):
    pts.sort()
    xs = [p[0] for p in pts]
    ys = [p[1] for p in pts]
    plt.plot(xs, ys, marker=m, linewidth=2, label=f'Error rate = {e:.2f}')

plt.xlabel('Channel Delay (ms)')
plt.ylabel('TCP Throughput (Mbps)')
plt.title('TCP Throughput vs Delay and Error Rate')
plt.grid(True, alpha=0.4)
plt.legend()
plt.tight_layout()
plt.savefig('figure_5_2.png', dpi=150)
print('Saved figure_5_2.png')
```

Run:
```bash
python3 plot_task1.py
```

### 8.6 Fill in the report tables

Open `task1_results.csv`. Pick rows to populate:

**Table 5.1** — filter where `errRate = 0.00`, use all 5 delay values
**Table 5.2** — filter where `delay_ms = 10`, use all 4 error rate values

### 8.7 Push to GitHub

Commit and push your branch, then open a Pull Request into `main`.

---

## 9. Phase 4b — Task 2: CSMA LAN (Xavier)

**Goal:** Measure how throughput and PDR degrade as more UDP senders contend on a shared 10-node CSMA LAN.

**What you produce:** `task2_csma.cc`, `task2_csma.xml`, `figure_6_2.png`, Screenshots 6.1 & 6.2, Tables 6.1 & 6.2, Section 6.

### 9.1 Create the source file

```bash
cd ~/ns-allinone-3.43/ns-3.43/scratch
nano task2_csma.cc
```

Paste:

```cpp
/*
 * TNN4113 Group 16 - Task 2
 * 10-node CSMA LAN: Shared-Medium Contention Study
 * Student: Xavier Liong Zhi Hao (86079)
 *
 * Topology:  10 nodes (N0..N9) on one CSMA channel
 *            N0 = sink (PacketSink / UDP)
 *            N1..NnSenders = OnOff/UDP senders, each at 5 Mbps CBR
 *
 * Varied:    nSenders in {1, 3, 5}
 *
 * Measured:  per-flow throughput, per-flow PDR,
 *            NetAnim XML for visualisation.
 */

#include "ns3/core-module.h"
#include "ns3/network-module.h"
#include "ns3/internet-module.h"
#include "ns3/csma-module.h"
#include "ns3/applications-module.h"
#include "ns3/flow-monitor-module.h"
#include "ns3/netanim-module.h"
#include "ns3/ipv4-flow-classifier.h"

using namespace ns3;

NS_LOG_COMPONENT_DEFINE("Task2CSMA");

int main(int argc, char *argv[])
{
    uint32_t nSenders = 1;
    uint32_t simTime  = 20;

    CommandLine cmd;
    cmd.AddValue("nSenders", "Number of active senders (1, 3, or 5)", nSenders);
    cmd.AddValue("simTime",  "Simulation time in seconds",             simTime);
    cmd.Parse(argc, argv);

    NodeContainer lan;
    lan.Create(10);

    CsmaHelper csma;
    csma.SetChannelAttribute("DataRate", StringValue("100Mbps"));
    csma.SetChannelAttribute("Delay",    TimeValue(NanoSeconds(6560)));

    NetDeviceContainer devs = csma.Install(lan);

    InternetStackHelper stack;
    stack.Install(lan);

    Ipv4AddressHelper addr;
    addr.SetBase("10.2.1.0", "255.255.255.0");
    Ipv4InterfaceContainer ifs = addr.Assign(devs);

    uint16_t port = 7;
    PacketSinkHelper sink("ns3::UdpSocketFactory",
                          InetSocketAddress(Ipv4Address::GetAny(), port));
    ApplicationContainer sinkApp = sink.Install(lan.Get(0));
    sinkApp.Start(Seconds(0.0));
    sinkApp.Stop(Seconds(simTime));

    for (uint32_t i = 1; i <= nSenders; ++i) {
        OnOffHelper on("ns3::UdpSocketFactory",
                       InetSocketAddress(ifs.GetAddress(0), port));
        on.SetAttribute("DataRate",   StringValue("5Mbps"));
        on.SetAttribute("PacketSize", UintegerValue(1024));
        on.SetAttribute("OnTime",
            StringValue("ns3::ConstantRandomVariable[Constant=1]"));
        on.SetAttribute("OffTime",
            StringValue("ns3::ConstantRandomVariable[Constant=0]"));
        ApplicationContainer a = on.Install(lan.Get(i));
        a.Start(Seconds(1.0 + 0.01 * i));
        a.Stop(Seconds(simTime));
    }

    AnimationInterface anim("task2_csma.xml");
    for (uint32_t i = 0; i < lan.GetN(); ++i) {
        anim.SetConstantPosition(lan.Get(i), i * 5.0, 10.0);
        anim.UpdateNodeDescription(lan.Get(i),
            (i == 0) ? "SINK-N0" : ("N" + std::to_string(i)));
    }

    FlowMonitorHelper fmh;
    Ptr<FlowMonitor> fm = fmh.InstallAll();

    Simulator::Stop(Seconds(simTime));
    Simulator::Run();

    fm->CheckForLostPackets();
    auto cs    = DynamicCast<Ipv4FlowClassifier>(fmh.GetClassifier());
    auto stats = fm->GetFlowStats();

    std::cout << "\n=== Task 2 results (nSenders=" << nSenders << ") ===\n";
    double aggThr = 0.0;
    for (auto &kv : stats) {
        auto t = cs->FindFlow(kv.first);
        double thrMbps = kv.second.rxBytes * 8.0 / ((simTime - 1) * 1e6);
        double pdr = (kv.second.txPackets == 0) ? 0.0 :
                     100.0 * kv.second.rxPackets / kv.second.txPackets;
        std::cout << "Flow "   << kv.first
                  << "  "     << t.sourceAddress << " -> " << t.destinationAddress
                  << "  Thr=" << thrMbps << " Mbps"
                  << "  Tx="  << kv.second.txPackets
                  << "  Rx="  << kv.second.rxPackets
                  << "  PDR=" << pdr << "%\n";
        aggThr += thrMbps;
    }
    std::cout << "AGGREGATE Throughput at sink = " << aggThr << " Mbps\n";

    Simulator::Destroy();
    return 0;
}
```

### 9.2 Build and test

```bash
cd ~/ns-allinone-3.43/ns-3.43
./ns3 build
./ns3 run "scratch/task2_csma --nSenders=1"
```

### 9.3 Run all three scenarios

```bash
./ns3 run "scratch/task2_csma --nSenders=1" | tee task2_n1.log
./ns3 run "scratch/task2_csma --nSenders=3" | tee task2_n3.log
./ns3 run "scratch/task2_csma --nSenders=5" | tee task2_n5.log
```

After the 5-sender run, save the XML:

```bash
cp task2_csma.xml task2_csma_5senders.xml
```

### 9.4 Open NetAnim and take Screenshot 6.1

```bash
NetAnim
```

1. File → open `task2_csma.xml`
2. Zoom out so all 10 nodes are visible in a row
3. Press Play
4. Let it animate for ~2 seconds then pause
5. Screenshot while frame-exchange arrows are visible between senders and SINK-N0
6. Save as `6.1_netanim.png`

### 9.5 Take Screenshot 6.2

Screenshot your terminal showing the 5-sender FlowMonitor output (per-flow Throughput + PDR + AGGREGATE line). Save as `6.2_task2_terminal.png`.

### 9.6 Plot Figure 6.2

Fill in your real numbers from the log files, then run:

```python
#!/usr/bin/env python3
"""plot_task2.py — Figure 6.2: CSMA aggregate throughput and PDR.
TNN4113 Group 16 — Xavier Liong Zhi Hao (86079)
"""
import matplotlib.pyplot as plt
import numpy as np

# Replace these with your real values from task2_n1/n3/n5.log
senders  = [1, 3, 5]
agg_thr  = [4.99, 14.85, 24.00]   # REPLACE with real AGGREGATE lines
mean_pdr = [100.0, 99.0, 96.0]    # REPLACE with real mean PDR per scenario

fig, ax1 = plt.subplots(figsize=(8, 5))
x = np.arange(len(senders))
w = 0.35

ax1.bar(x - w/2, agg_thr, w, color='steelblue', label='Aggregate Throughput (Mbps)')
ax1.set_xlabel('Number of Active Senders')
ax1.set_ylabel('Aggregate Throughput (Mbps)', color='steelblue')
ax1.tick_params(axis='y', labelcolor='steelblue')
ax1.set_xticks(x)
ax1.set_xticklabels(senders)

ax2 = ax1.twinx()
ax2.bar(x + w/2, mean_pdr, w, color='indianred', label='Mean PDR (%)')
ax2.set_ylabel('Mean PDR (%)', color='indianred')
ax2.tick_params(axis='y', labelcolor='indianred')
ax2.set_ylim(0, 105)

plt.title('Aggregate Throughput and PDR vs Number of CSMA Senders')
fig.tight_layout()
plt.savefig('figure_6_2.png', dpi=150)
print('Saved figure_6_2.png')
```

### 9.7 Push to GitHub

Commit and push your branch, then open a Pull Request into `main`.

---

## 10. Phase 4c — Task 3: Dumbbell + TCP (Clarence)

**Goal:** Compare TCP NewReno vs TCP Cubic congestion-window behaviour over a shared 1 Mbps bottleneck.

**What you produce:** `task3_dumbbell.cc`, `cwnd-newreno.dat`, `cwnd-cubic.dat`, `cwnd_comparison.png`, Screenshot 7.1, Table 7.1, Section 7.

### 10.1 Create the source file

```bash
cd ~/ns-allinone-3.43/ns-3.43/scratch
nano task3_dumbbell.cc
```

Paste:

```cpp
/*
 * TNN4113 Group 16 - Task 3
 * Dumbbell Topology: TCP NewReno vs TCP Cubic over a 1 Mbps Bottleneck
 * Student: Ng Clarence Chuan Hann (84832)
 *
 *      L0 ----\                                        /---- R0
 *              left-router --- 1Mbps/20ms --- right-router
 *      L1 ----/    DropTail queue, 50 packets          \---- R1
 *
 *   L0 -> R0 : TCP NewReno (BulkSend)
 *   L1 -> R1 : TCP Cubic   (BulkSend)
 *
 *   cWnd of each sender traced to .dat files on every change.
 */

#include "ns3/core-module.h"
#include "ns3/network-module.h"
#include "ns3/internet-module.h"
#include "ns3/point-to-point-module.h"
#include "ns3/point-to-point-layout-module.h"
#include "ns3/applications-module.h"
#include "ns3/traffic-control-module.h"

#include <fstream>
#include <string>

using namespace ns3;

NS_LOG_COMPONENT_DEFINE("Task3Dumbbell");

static std::ofstream g_renoFile;
static std::ofstream g_cubicFile;

static void CwndTracerReno(uint32_t /*oldVal*/, uint32_t newVal)
{
    g_renoFile << Simulator::Now().GetSeconds() << "\t" << newVal << "\n";
}

static void CwndTracerCubic(uint32_t /*oldVal*/, uint32_t newVal)
{
    g_cubicFile << Simulator::Now().GetSeconds() << "\t" << newVal << "\n";
}

int main(int argc, char *argv[])
{
    uint32_t simTime = 30;
    CommandLine cmd;
    cmd.AddValue("simTime", "Simulation time in seconds", simTime);
    cmd.Parse(argc, argv);

    g_renoFile.open("cwnd-newreno.dat");
    g_cubicFile.open("cwnd-cubic.dat");

    PointToPointHelper accessLink;
    accessLink.SetDeviceAttribute("DataRate", StringValue("10Mbps"));
    accessLink.SetChannelAttribute("Delay",   StringValue("1ms"));

    PointToPointHelper bottleneck;
    bottleneck.SetDeviceAttribute("DataRate", StringValue("1Mbps"));
    bottleneck.SetChannelAttribute("Delay",   StringValue("20ms"));
    bottleneck.SetQueue("ns3::DropTailQueue",
                        "MaxSize", StringValue("50p"));

    PointToPointDumbbellHelper db(2, accessLink, 2, accessLink, bottleneck);

    InternetStackHelper stack;
    db.InstallStack(stack);

    db.AssignIpv4Addresses(
        Ipv4AddressHelper("10.1.0.0", "255.255.255.0"),
        Ipv4AddressHelper("10.2.0.0", "255.255.255.0"),
        Ipv4AddressHelper("10.3.0.0", "255.255.255.0"));

    TypeId tidReno  = TypeId::LookupByName("ns3::TcpNewReno");
    TypeId tidCubic = TypeId::LookupByName("ns3::TcpCubic");

    Config::Set("/NodeList/" + std::to_string(db.GetLeft(0)->GetId()) +
                "/$ns3::TcpL4Protocol/SocketType", TypeIdValue(tidReno));
    Config::Set("/NodeList/" + std::to_string(db.GetLeft(1)->GetId()) +
                "/$ns3::TcpL4Protocol/SocketType", TypeIdValue(tidCubic));

    uint16_t port = 5001;

    for (uint32_t i = 0; i < 2; ++i) {
        PacketSinkHelper sink("ns3::TcpSocketFactory",
            InetSocketAddress(Ipv4Address::GetAny(), port));
        ApplicationContainer s = sink.Install(db.GetRight(i));
        s.Start(Seconds(0.0));
        s.Stop(Seconds(simTime));
    }

    for (uint32_t i = 0; i < 2; ++i) {
        BulkSendHelper src("ns3::TcpSocketFactory",
            InetSocketAddress(db.GetRightIpv4Address(i), port));
        src.SetAttribute("MaxBytes", UintegerValue(0));
        ApplicationContainer a = src.Install(db.GetLeft(i));
        a.Start(Seconds(1.0));
        a.Stop(Seconds(simTime));
    }

    Simulator::Schedule(Seconds(1.001), [&]() {
        Config::ConnectWithoutContext(
            "/NodeList/" + std::to_string(db.GetLeft(0)->GetId()) +
            "/$ns3::TcpL4Protocol/SocketList/0/CongestionWindow",
            MakeCallback(&CwndTracerReno));
        Config::ConnectWithoutContext(
            "/NodeList/" + std::to_string(db.GetLeft(1)->GetId()) +
            "/$ns3::TcpL4Protocol/SocketList/0/CongestionWindow",
            MakeCallback(&CwndTracerCubic));
    });

    Simulator::Stop(Seconds(simTime));
    Simulator::Run();
    Simulator::Destroy();

    g_renoFile.close();
    g_cubicFile.close();

    std::cout << "Task 3 complete. Output files:\n"
              << "  cwnd-newreno.dat\n"
              << "  cwnd-cubic.dat\n";
    return 0;
}
```

### 10.2 Build and run

```bash
cd ~/ns-allinone-3.43/ns-3.43
./ns3 build
./ns3 run scratch/task3_dumbbell
```

Verify the output files exist:

```bash
ls -l cwnd-newreno.dat cwnd-cubic.dat
head cwnd-newreno.dat
head cwnd-cubic.dat
```

Both should be non-empty with two columns: `time_seconds   cwnd_bytes`.

### 10.3 Take Screenshot 7.1

Screenshot your terminal showing the `./ns3 run` completion message and the `ls -l` output confirming both `.dat` files. Save as `7.1_task3_run.png`.

### 10.4 Plot Figure 7.2 (cWnd comparison)

**Using Python (recommended):**

```python
#!/usr/bin/env python3
"""plot_cwnd.py — Figure 7.2: cWnd Evolution NewReno vs Cubic.
TNN4113 Group 16 — Ng Clarence Chuan Hann (84832)
"""
import numpy as np
import matplotlib.pyplot as plt

reno  = np.loadtxt('cwnd-newreno.dat')
cubic = np.loadtxt('cwnd-cubic.dat')

plt.figure(figsize=(11, 6))
plt.plot(reno[:, 0],  reno[:, 1],  label='TCP NewReno', linewidth=1.6)
plt.plot(cubic[:, 0], cubic[:, 1], label='TCP Cubic',   linewidth=1.6)
plt.xlabel('Time (s)')
plt.ylabel('Congestion Window (bytes)')
plt.title('cWnd Evolution: NewReno vs Cubic over a 1 Mbps Bottleneck')
plt.grid(True, alpha=0.4)
plt.legend(loc='upper left')
plt.tight_layout()
plt.savefig('cwnd_comparison.png', dpi=150)
print('Saved cwnd_comparison.png')
```

**Using gnuplot (alternative):**

```bash
cat > plot_cwnd.gp << 'EOF'
set terminal pngcairo size 1100,600 enhanced font 'Arial,11'
set output 'cwnd_comparison.png'
set title 'cWnd Evolution: NewReno vs Cubic over a 1 Mbps Bottleneck'
set xlabel 'Time (s)'
set ylabel 'Congestion Window (bytes)'
set key top left
set grid
plot 'cwnd-newreno.dat' using 1:2 with lines lw 2 title 'TCP NewReno', \
     'cwnd-cubic.dat'   using 1:2 with lines lw 2 title 'TCP Cubic'
EOF
gnuplot plot_cwnd.gp
```

### 10.5 Extract Table 7.1 — Drop & Recovery events

Run this Python script to auto-detect the first 3 congestion drop events for each variant:

```python
import numpy as np

for label, fn in [('NewReno', 'cwnd-newreno.dat'), ('Cubic', 'cwnd-cubic.dat')]:
    d = np.loadtxt(fn)
    t, cwnd = d[:, 0], d[:, 1]
    drops = []
    for i in range(1, len(cwnd)):
        if cwnd[i] < cwnd[i-1] * 0.95:
            w_max  = cwnd[i-1]
            w_min  = cwnd[i]
            t_drop = t[i]
            recovery_time = None
            for j in range(i+1, len(cwnd)):
                if cwnd[j] >= w_max:
                    recovery_time = t[j] - t_drop
                    break
            drops.append((t_drop, w_max, w_min, recovery_time))
            if len(drops) == 3:
                break
    print(f"\n=== {label} ===")
    for k, (td, wmx, wmn, rt) in enumerate(drops, 1):
        rt_str = f"{rt:.3f}s" if rt is not None else "N/A"
        print(f"  Drop {k}: t={td:.3f}s  W_max={int(wmx)}  W_min={int(wmn)}  Recovery={rt_str}")
```

Copy the output numbers directly into Table 7.1 of the report.

### 10.6 Cross-check your analysis

Look at your `cwnd_comparison.png` and verify:
- NewReno shows a classic linear sawtooth pattern ✓
- Cubic reaches higher cWnd values between drops ✓
- Cubic's recovery time in Table 7.1 is shorter than NewReno's ✓

If both curves look identical — the per-node TCP variant assignment silently failed. Print the node IDs to debug:
```cpp
std::cout << "Left 0 ID = " << db.GetLeft(0)->GetId() << "\n";
std::cout << "Left 1 ID = " << db.GetLeft(1)->GetId() << "\n";
```

### 10.7 Push to GitHub

Commit and push your branch, then open a Pull Request into `main`.

---

## 11. Phase 5 — Report Assembly (Ming Zin)

You do not run any simulation. Your job is to produce the final, complete, submission-ready report.

### 11.1 While others are running simulations (Days 1–3)

Draft these sections — they do not require any simulation data:

- **Section 1** — Executive Summary (leave result sentences as placeholders for now)
- **Section 2** — Introduction (already written in template, fill names and group number)
- **Section 3** — Background and Theoretical Framework (already complete in template)
- **Section 4** — Methodology — **update the environment table to Ubuntu 24.04 / g++ 13.3.0 / Python 3.12.3 / gnuplot 6.0** as shown at the top of this guide

### 11.2 Draw the 3 topology diagrams

Go to **https://app.diagrams.net** (free, no login needed).

**Figure 5.1 — P2P Topology:**
- Two circles: "Node 0 (TCP Source / BulkSend)" and "Node 1 (TCP Sink)"
- One connecting line labelled: "5 Mbps · variable delay · variable error rate"

**Figure 6.1 — CSMA Topology:**
- Ten circles in a row labelled N0 through N9
- One horizontal line underneath all of them (the shared channel)
- N0 coloured differently, labelled "SINK"
- Channel labelled: "100 Mbps, 6560 ns"

**Figure 7.1 — Dumbbell Topology:**
- Left side: L0, L1 connecting to "Left Router"
- Middle: "Left Router" → "Right Router" labelled "1 Mbps / 20 ms / DropTail 50p"
- Right side: "Right Router" → R0, R1
- Access links labelled: "10 Mbps / 1 ms"

Export each as PNG and insert into the report.

### 11.3 Day 4 — Collect and merge

Once all 3 teammates have merged their PRs into `main`, pull the latest:

```bash
git checkout main
git pull
```

Then open `TNN4113_Project_Report.docx`.
3. Replace every `[INSERT]` and `>>> PLACEHOLDER <<<` using this checklist:

| Find | Replace with |
|---|---|
| `[INSERT NAME]` × 8 | Brendan, Xavier, Clarence, Ming Zin (real full names) |
| `[INSERT ID]` × 4 | 83403, 86079, 84832, 83489 |
| `>>> PLACEHOLDER: INSERT FIGURE 5.1 <<<` | Figure 5.1 PNG |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 5.1 <<<` | `5.1_task1_run.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 5.2 <<<` | `figure_5_2.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 6.1 <<<` | Figure 6.1 PNG |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 6.1 <<<` | `6.1_netanim.png` |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 6.2 <<<` | `6.2_task2_terminal.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 6.2 <<<` | `figure_6_2.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 7.1 <<<` | Figure 7.1 PNG |
| `>>> PLACEHOLDER: INSERT FIGURE 7.2 <<<` | `cwnd_comparison.png` |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 7.1 <<<` | `7.1_task3_run.png` |
| All `[INSERT]` cells in Tables 5.1, 5.2, 6.1, 6.2, 7.1 | Real numbers from CSV / logs |

Use Word's **Find & Replace (Ctrl+H)** — search for `[INSERT` — must return zero results when done.

### 11.4 Write Section 8 (Cross-Task Synthesis)

Verify the three-layer summary table matches the real results:
- Task 1: "Throughput ∝ 1/RTT and ≈1/√p" — confirmed by Tables 5.1/5.2?
- Task 2: "PDR drops with each new sender" — confirmed by Table 6.1?
- Task 3: "Cubic more aggressive, recovers faster" — confirmed by Table 7.1?

Update any text that contradicts the real numbers.

### 11.5 Fill Peer Evaluation (Section 10)

Fill in real names and matric IDs. Adjust contribution percentages if the workload was uneven. Everyone signs.

### 11.6 Assemble submission package

```bash
mkdir -p ~/Group16_Submission/source_code
mkdir -p ~/Group16_Submission/trace_outputs
mkdir -p ~/Group16_Submission/plots
mkdir -p ~/Group16_Submission/screenshots

cp task1_p2p.cc task2_csma.cc task3_dumbbell.cc  ~/Group16_Submission/source_code/
cp task1_results.csv task2_csma.xml               ~/Group16_Submission/trace_outputs/
cp cwnd-newreno.dat cwnd-cubic.dat                ~/Group16_Submission/trace_outputs/
cp figure_5_2.png figure_6_2.png cwnd_comparison.png ~/Group16_Submission/plots/
cp 5.1_task1_run.png 6.1_netanim.png \
   6.2_task2_terminal.png 7.1_task3_run.png       ~/Group16_Submission/screenshots/
cp TNN4113_Project_Report.docx                    ~/Group16_Submission/
cp TNN4113_Project_Report.pdf                     ~/Group16_Submission/

cd ~
zip -r Group16_TNN4113_Submission.zip Group16_Submission/
```

---

## 12. Phase 6 — Final Review & Submission

### 12.1 Group review meeting (Day 5 morning)

Video call, screen-share the PDF:
- Brendan reads Sections 6, 7, 8
- Xavier reads Sections 5, 7, 8
- Clarence reads Sections 5, 6, 8
- Ming Zin moderates and applies fixes live

### 12.2 Upload to eLeap

1. Log in to eLeap
2. Navigate to TNN4113 project submission
3. Upload `Group16_TNN4113_Submission.zip`
4. Confirm upload success
5. Screenshot the confirmation page

**Do this before 5 PM on 21 May — never at the last minute.**

---

## 13. Troubleshooting

**`E: Unable to locate package sqlite`**
Remove `sqlite` from the apt command. `sqlite3` and `libsqlite3-dev` are sufficient — both are already in the Section 4.4 command.

**`E: Package 'qtchooser' has no installation candidate`**
`qtchooser` was dropped from Ubuntu 24.04. Remove it from the apt command — it is already absent from the Section 4.4 command.

**`error: externally-managed-environment` when using pip3**
Ubuntu 24.04 blocks pip installs into the system Python. Use apt instead:
```bash
sudo apt install python3-matplotlib python3-numpy -y
```
This is already how Section 4.4 installs them.

**NS-3.43 build fails with C++ errors inside NS-3 source files**
g++ 13 is stricter than g++ 11. Apply this patch before building:
```bash
cd ~/ns-allinone-3.43/ns-3.43
find . -name "*.cc" -o -name ".h" | xargs grep -l "uint8_t\|uint32_t" | head -5
```
If the build fails on a specific file, the most common fix is:
```bash
./ns3 configure --enable-examples --enable-tests \
  --CXX_FLAGS="-Wno-error=deprecated-declarations -Wno-error=unused-variable"
./ns3 build
```

**`./ns3 run hello-simulator` gives an error**
Re-run configure:
```bash
./ns3 clean
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

**`cwnd-newreno.dat` is empty after Task 3 run**
Push the schedule to `Seconds(1.5)`:
```cpp
Simulator::Schedule(Seconds(1.5), [&]() { ... });
```

**NetAnim window does not open**
Make sure VirtualBox Guest Additions is installed — it provides the display drivers needed for GUI apps:
```bash
sudo apt install -y virtualbox-guest-x11
reboot
```

**Both cWnd curves in Task 3 look identical**
Print node IDs to confirm the Config::Set assignments are correct:
```cpp
std::cout << "Left 0 ID = " << db.GetLeft(0)->GetId() << "\n";
std::cout << "Left 1 ID = " << db.GetLeft(1)->GetId() << "\n";
```

**Task 1 sweep script hangs on one combination**
Abort with Ctrl+C and run that combination manually:
```bash
./ns3 run "scratch/task1_p2p --delay=60ms --errRate=0.05"
```

---

## 14. Submission Checklist

### Source code
- [ ] `task1_p2p.cc` — compiles on NS-3.43, runs cleanly
- [ ] `task2_csma.cc` — compiles on NS-3.43, runs cleanly
- [ ] `task3_dumbbell.cc` — compiles on NS-3.43, runs cleanly
- [ ] All 3 files have header comment with group number, task, and student name

### Data outputs
- [ ] `task1_results.csv` — 20 rows of real simulation data
- [ ] `task2_csma.xml` — generated from 5-sender run, opens in NetAnim
- [ ] `cwnd-newreno.dat` — non-empty, two columns
- [ ] `cwnd-cubic.dat` — non-empty, two columns

### Plots (generated output, not screenshots)
- [ ] `figure_5_2.png` — Throughput vs Delay & Error Rate, 4 curves
- [ ] `figure_6_2.png` — CSMA aggregate throughput & PDR bar chart
- [ ] `cwnd_comparison.png` — NewReno vs Cubic cWnd over 30 s

### Screenshots (4 mandatory)
- [ ] `5.1_task1_run.png` — Task 1 terminal run
- [ ] `6.1_netanim.png` — NetAnim CSMA animation with arrows
- [ ] `6.2_task2_terminal.png` — Task 2 FlowMonitor 5-sender output
- [ ] `7.1_task3_run.png` — Task 3 terminal + `ls -l` of .dat files

### Topology diagrams (hand-drawn / draw.io)
- [ ] Figure 5.1 — P2P topology
- [ ] Figure 6.1 — 10-node CSMA LAN
- [ ] Figure 7.1 — Dumbbell topology

### Report
- [ ] Section 4.1 environment table updated to Ubuntu 24.04 / g++ 13.3.0 / Python 3.12.3 / gnuplot 6.0
- [ ] All 4 names + matric IDs filled — Brendan (83403), Xavier (86079), Clarence (84832), Ming Zin (83489)
- [ ] Zero remaining `[INSERT]` cells in any table
- [ ] Zero remaining `>>> PLACEHOLDER <<<` blocks
- [ ] Peer Evaluation signed by all 4 members
- [ ] PDF exported

### Submission
- [ ] ZIP named `Group16_TNN4113_Submission.zip`
- [ ] Uploaded to eLeap before 21 May 2026 deadline
- [ ] Confirmation screenshot saved
- [ ] All members have a personal backup copy

---

## Quick Command Reference

```bash
# Everyone — verify NS-3.43
cd ~/ns-allinone-3.43/ns-3.43 && ./ns3 show version

# Brendan (S1) — single test run
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.00"

# Brendan (S1) — full 20-combo sweep
./task1_sweep.sh

# Xavier (S2) — all three CSMA scenarios
for n in 1 3 5; do
  ./ns3 run "scratch/task2_csma --nSenders=$n" | tee task2_n${n}.log
done

# Xavier (S2) — open NetAnim
NetAnim

# Clarence (S3) — run dumbbell + plot
./ns3 run scratch/task3_dumbbell && python3 plot_cwnd.py

# Ming Zin (S4) — assemble zip
cd ~ && zip -r Group16_TNN4113_Submission.zip Group16_Submission/
```