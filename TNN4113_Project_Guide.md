# TNN4113 Computer Networks — Complete Project Execution Guide

**Project:** Network Performance Analysis Using NS-3 Simulator
**Course:** TNN4113 Computer Networks
**Submission Date:** 21 May 2026
**Team Size:** 4 students
**Group:** Group 4

---

## ⚠️ Mandatory Environment Specification

This is taken **directly from your project report template (Section 4.1 — Methodology and Common Setup)**. Every member must match these versions exactly.

| Item | Version / Value |
|---|---|
| Operating System | Ubuntu 22.04 LTS (under WSL2 / native Linux) |
| ns-3 release | **ns-3.40** |
| Compiler | g++ 11.4 with C++17 |
| Build system | CMake via ./ns3 wrapper |
| NetAnim version | **3.108** |
| Plotting tool | gnuplot 5.4 / Python 3.10 + Matplotlib 3.7 |
| Random-number seed | RngSeedManager::SetSeed(1) |
| Run number | RngRun = 1 (varied 1–5 for repeatability checks) |

> **Why 3.40 specifically?** The latest NS-3 version as of 2025 is 3.47, but your project brief explicitly specifies 3.40. All source code in this guide was written and tested for 3.40. Do not use any other version — using a different version risks API mismatches and will contradict what you declare in your submitted report.

---

## Table of Contents

1. [Team Roles & Responsibilities](#1-team-roles--responsibilities)
2. [Recommended Timeline (5 Days)](#2-recommended-timeline-5-days)
3. [File Sharing — Google Drive (No Git Needed)](#3-file-sharing--google-drive-no-git-needed)
4. [Phase 0 — System Setup (Day 1, Everyone)](#4-phase-0--system-setup-day-1-everyone)
5. [Phase 1 — Install NS-3.40 (Day 1, Everyone)](#5-phase-1--install-ns-340-day-1-everyone)
6. [Phase 2 — Verify Environment (Day 1, Everyone)](#6-phase-2--verify-environment-day-1-everyone)
7. [Phase 3 — Install NetAnim 3.108 (Day 1, Student 2 mandatory)](#7-phase-3--install-netanim-3108-day-1-student-2-mandatory)
8. [Phase 4a — Task 1: Point-to-Point (Student 1)](#8-phase-4a--task-1-point-to-point-student-1)
9. [Phase 4b — Task 2: CSMA LAN (Student 2)](#9-phase-4b--task-2-csma-lan-student-2)
10. [Phase 4c — Task 3: Dumbbell + TCP (Student 3)](#10-phase-4c--task-3-dumbbell--tcp-student-3)
11. [Phase 5 — Report Assembly (Student 4)](#11-phase-5--report-assembly-student-4)
12. [Phase 6 — Final Review & Submission](#12-phase-6--final-review--submission)
13. [Troubleshooting](#13-troubleshooting)
14. [Submission Checklist](#14-submission-checklist)

---

## 1. Team Roles & Responsibilities

| Student | Role | Simulation File | Key Deliverables |
|---|---|---|---|
| **Student 1** | Task 1 — Point-to-Point | `task1_p2p.cc` | `task1_results.csv`, Tables 5.1 & 5.2, Figure 5.2, Screenshot 5.1, Section 5 |
| **Student 2** | Task 2 — CSMA LAN | `task2_csma.cc` | `task2_csma.xml`, Tables 6.1 & 6.2, Figure 6.2, Screenshots 6.1 & 6.2, Section 6 |
| **Student 3** | Task 3 — Dumbbell TCP | `task3_dumbbell.cc` | `cwnd-newreno.dat`, `cwnd-cubic.dat`, `cwnd_comparison.png`, Table 7.1, Screenshot 7.1, Section 7 |
| **Student 4** | Task 4 — Synthesis & Report | None | Topology diagrams (Figs 5.1/6.1/7.1), Sections 1–4 & 8–12, final report merge, submission zip |

**Important:** Each student runs their own simulation **independently on their own laptop**. There is no shared simulation — you each have a different `.cc` file and generate different output files. Only at the end does Student 4 collect everyone's outputs and assemble the final package.

---

## 2. Recommended Timeline (5 Days)

| Day | Date | Student 1 | Student 2 | Student 3 | Student 4 |
|---|---|---|---|---|---|
| **Day 1 — Sat 17 May** | Install NS-3.40, get `task1_p2p.cc` compiling | Install NS-3.40 + NetAnim 3.108, get `task2_csma.cc` compiling | Install NS-3.40, get `task3_dumbbell.cc` compiling | Set up Google Drive folder, draft Sections 1–4, start topology diagrams |
| **Day 2 — Sun 18 May** | Run 20-combo sweep, generate CSV + Figure 5.2 | Run 1/3/5 sender scenarios, open NetAnim, take screenshots | Run dumbbell, generate .dat files, plot cWnd | Continue Sections 1–4, draw Figures 5.1/6.1/7.1 in draw.io |
| **Day 3 — Mon 19 May** | Fill Tables 5.1/5.2, write Section 5 analysis | Fill Tables 6.1/6.2, write Section 6 analysis | Fill Table 7.1, write Section 7 analysis | Draft Section 8 (Cross-Task Synthesis) |
| **Day 4 — Tue 20 May** | Upload all files to Drive, review merged doc | Upload all files to Drive, review merged doc | Upload all files to Drive, review merged doc | Merge everyone's work into report, replace ALL `[INSERT]` placeholders, write Sections 9/10 |
| **Day 5 — Wed 21 May** | Final check of Section 5 in merged report | Final check of Section 6 in merged report | Final check of Section 7 in merged report | Export PDF, assemble zip, upload to eLeap before deadline |

---

## 3. File Sharing — Google Drive (No Git Needed)

You do not need GitHub. Create one shared Google Drive folder and use this structure:

```
Group4_TNN4113/
├── src/
│   ├── task1_p2p.cc          ← Student 1 uploads here
│   ├── task2_csma.cc          ← Student 2 uploads here
│   └── task3_dumbbell.cc      ← Student 3 uploads here
├── results/
│   ├── task1_results.csv      ← Student 1
│   ├── task2_csma.xml         ← Student 2 (5-sender run)
│   ├── cwnd-newreno.dat       ← Student 3
│   ├── cwnd-cubic.dat         ← Student 3
│   ├── figure_5_2.png         ← Student 1
│   ├── figure_6_2.png         ← Student 2
│   └── cwnd_comparison.png    ← Student 3
├── screenshots/
│   ├── 5.1_task1_run.png      ← Student 1
│   ├── 6.1_netanim.png        ← Student 2
│   ├── 6.2_task2_terminal.png ← Student 2
│   └── 7.1_task3_run.png      ← Student 3
└── report/
    └── TNN4113_Project_Report.docx  ← Student 4 owns this
```

Student 4 creates the folder on Day 1 and shares it with all members. Each student uploads their files as they complete them — don't wait until Day 4.

---

## 4. Phase 0 — System Setup (Day 1, Everyone)

### 4.1 If you are on Windows — Install WSL2 + Ubuntu 22.04

Open **PowerShell as Administrator:**

```powershell
wsl --install -d Ubuntu-22.04
```

Reboot when prompted. Open Ubuntu from the Start menu, create a UNIX username and password. Then update WSL:

```powershell
wsl --update
```

### 4.2 If you are on native Ubuntu 22.04 — Skip to 4.3

### 4.3 Update your system

```bash
sudo apt update && sudo apt upgrade -y
```

### 4.4 Install all required packages

The following command has been corrected from the original — `sqlite` (which does not exist as a standalone package on Ubuntu 22.04) has been removed, and `libgcrypt-dev` has been replaced with `libgcrypt20-dev` which is the correct package name on this OS version:

```bash
sudo apt install -y \
  build-essential cmake ninja-build pkg-config \
  g++ gdb gcc-multilib g++-multilib \
  python3 python3-dev python3-pip python3-setuptools \
  git mercurial \
  qtbase5-dev qttools5-dev qttools5-dev-tools qtchooser qt5-qmake \
  mpi-default-bin mpi-default-dev openmpi-bin openmpi-common openmpi-doc libopenmpi-dev \
  autoconf automake libxml2 libxml2-dev libgcrypt20-dev libgsl-dev \
  flex bison libfl-dev tcpdump sqlite3 libsqlite3-dev \
  libgtk-3-dev gnuplot wget tar bzip2 unzip
```

> **Note on sqlite:** `sqlite3` and `libsqlite3-dev` are included above and are sufficient. The standalone `sqlite` package does not exist on Ubuntu 22.04 — ignore any guides that list it. NS-3 does not require it for any of your three tasks.

Then install Python plotting libraries:

```bash
pip3 install --user matplotlib numpy
```

### 4.5 Verify toolchain

```bash
g++ --version        # must show 11.x
python3 --version    # must show 3.10.x
cmake --version      # must show 3.22 or newer
qmake --version      # must show Qt 5.15.x
gnuplot --version    # must show 5.4
```

---

## 5. Phase 1 — Install NS-3.40 (Day 1, Everyone)

### 5.1 Download the official NS-3.40 tarball

```bash
cd ~
wget https://www.nsnam.org/releases/ns-allinone-3.40.tar.bz2
```

> This is the only correct download URL for NS-3.40. Do not download any other version. As of 2025 the latest NS-3 version is 3.47, but your project report explicitly specifies **ns-3.40** in Section 4.1 and the source code in this guide is written for 3.40.

### 5.2 Extract and build

```bash
tar xjf ns-allinone-3.40.tar.bz2
cd ns-allinone-3.40
./build.py --enable-examples --enable-tests
```

This takes **15–40 minutes** depending on your machine. Do not interrupt it. You will see many compiler lines scroll past — that is normal.

When it finishes you will see:

```
Build finished successfully.
```

Your NS-3 root directory is:

```
~/ns-allinone-3.40/ns-3.40/
```

All simulation commands from this point are run from inside that directory.

### 5.3 Alternative — Git clone (use only if tarball download fails)

```bash
cd ~
git clone https://gitlab.com/nsnam/ns-3-dev.git ns-3.40
cd ns-3.40
git checkout ns-3.40
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

---

## 6. Phase 2 — Verify Environment (Day 1, Everyone)

All 4 members must run these checks and confirm the outputs match before doing anything else. This ensures you are all on identical environments as required by Section 4.1 of the report.

### 6.1 Check NS-3 version

```bash
cd ~/ns-allinone-3.40/ns-3.40
./ns3 --version
```

Expected output:
```
ns-3.40
```

### 6.2 Run hello-simulator

```bash
./ns3 run hello-simulator
```

Expected output:
```
Hello Simulator
```

### 6.3 Run a full example

```bash
./ns3 run first
```

You should see TCP packet-exchange log lines. If this works, your NS-3 installation is fully functional.

### 6.4 Environment verification summary

Screenshot your terminal showing all three outputs above and share in your group chat. Once all 4 members have confirmed matching outputs, you are ready to proceed.

---

## 7. Phase 3 — Install NetAnim 3.108 (Day 1, Student 2 mandatory)

NetAnim is bundled inside the ns-allinone tarball. You only need to compile it.

### 7.1 Build NetAnim

```bash
cd ~/ns-allinone-3.40/netanim-3.108
make clean
qmake NetAnim.pro
make -j$(nproc)
```

A `NetAnim` executable will appear in that folder.

### 7.2 Launch NetAnim

**On native Ubuntu:**
```bash
./NetAnim
```

**On WSL2 (Windows 11):** WSLg supports Linux GUI apps natively. Just run:
```bash
./NetAnim
```

A window should appear. If it does not, run `wsl --update` in PowerShell and try again.

**On WSL2 (Windows 10):** Install VcXsrv on Windows, then:
```bash
export DISPLAY=:0
./NetAnim
```

### 7.3 Add NetAnim to your PATH (convenience)

```bash
echo 'export PATH="$HOME/ns-allinone-3.40/netanim-3.108:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Now you can just type `NetAnim` from anywhere.

---

## 8. Phase 4a — Task 1: Point-to-Point (Student 1)

**Goal:** Measure how channel delay and packet error rate degrade TCP throughput on a single P2P link.

**What you produce:** `task1_p2p.cc`, `task1_results.csv`, `figure_5_2.png`, Screenshot 5.1, Tables 5.1 & 5.2, Section 5.

### 8.1 Create the source file

```bash
cd ~/ns-allinone-3.40/ns-3.40/scratch
nano task1_p2p.cc
```

Paste the complete source code below:

```cpp
/*
 * TNN4113 - Task 1
 * Point-to-Point Network: Link Characteristic & Reliability Analysis
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
cd ~/ns-allinone-3.40/ns-3.40
./ns3 build
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.00"
```

You should see one output line like:
```
Flow 1  Throughput=4.XX Mbps  MeanDelay=XX ms  Lost=0  TxPackets=N  RxPackets=N
```

### 8.3 Take Screenshot 5.1

Take a screenshot of your terminal showing the `./ns3 build` success and the `Flow 1 Throughput=...` output line. Save as `5.1_task1_run.png`.

### 8.4 Run the 20-combination sweep

Create the sweep script in the NS-3 root:

```bash
cd ~/ns-allinone-3.40/ns-3.40
nano task1_sweep.sh
```

Paste:

```bash
#!/bin/bash
# task1_sweep.sh — run all 20 (delay, error-rate) combinations

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
"""plot_task1.py — Figure 5.2: TCP throughput vs channel delay and error rate."""
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

### 8.7 Upload to Google Drive

Upload to `Group4_TNN4113/src/`, `results/`, and `screenshots/` folders.

---

## 9. Phase 4b — Task 2: CSMA LAN (Student 2)

**Goal:** Measure how throughput and PDR degrade as more UDP senders contend on a shared 10-node CSMA LAN.

**What you produce:** `task2_csma.cc`, `task2_csma.xml`, `figure_6_2.png`, Screenshots 6.1 & 6.2, Tables 6.1 & 6.2, Section 6.

### 9.1 Create the source file

```bash
cd ~/ns-allinone-3.40/ns-3.40/scratch
nano task2_csma.cc
```

Paste:

```cpp
/*
 * TNN4113 - Task 2
 * 10-node CSMA LAN: Shared-Medium Contention Study
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
cd ~/ns-allinone-3.40/ns-3.40
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
import matplotlib.pyplot as plt
import numpy as np

# Replace with your real values from task2_n1/n3/n5.log
senders  = [1, 3, 5]
agg_thr  = [4.99, 14.85, 24.00]   # REPLACE with real aggregate throughput
mean_pdr = [100.0, 99.0, 96.0]    # REPLACE with real mean PDR

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

### 9.7 Upload to Google Drive

Upload all files to their respective folders.

---

## 10. Phase 4c — Task 3: Dumbbell + TCP (Student 3)

**Goal:** Compare TCP NewReno vs TCP Cubic congestion-window behaviour over a shared 1 Mbps bottleneck.

**What you produce:** `task3_dumbbell.cc`, `cwnd-newreno.dat`, `cwnd-cubic.dat`, `cwnd_comparison.png`, Screenshot 7.1, Table 7.1, Section 7.

### 10.1 Create the source file

```bash
cd ~/ns-allinone-3.40/ns-3.40/scratch
nano task3_dumbbell.cc
```

Paste:

```cpp
/*
 * TNN4113 - Task 3
 * Dumbbell Topology: TCP NewReno vs TCP Cubic over a 1 Mbps Bottleneck
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
cd ~/ns-allinone-3.40/ns-3.40
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
        if cwnd[i] < cwnd[i-1] * 0.95:     # >5% sudden drop = congestion event
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

If both curves look identical — the per-node TCP variant assignment silently failed. Re-read the `Config::Set` lines in the source and confirm the node IDs are correct.

### 10.7 Upload to Google Drive

Upload all files to their respective folders.

---

## 11. Phase 5 — Report Assembly (Student 4)

You do not run any simulation. Your job is to produce the final, complete, submission-ready report.

### 11.1 While others are running simulations (Days 1–3)

Draft these sections — they do not require any simulation data:

- **Section 1** — Executive Summary (leave result sentences as placeholders for now)
- **Section 2** — Introduction (already written in template, fill names)
- **Section 3** — Background and Theoretical Framework (already complete in template)
- **Section 4** — Methodology (fill the environment table using the spec at the top of this guide)

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

Once all 3 teammates have uploaded to Google Drive:

1. Download all files from the Drive folder
2. Open `TNN4113_Project_Report.docx`
3. Replace every `[INSERT]` and `>>> PLACEHOLDER <<<` using this checklist:

| Find | Replace with |
|---|---|
| `[INSERT NAME]` × 8 | Real student names |
| `[INSERT ID]` × 4 | Real matric IDs |
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

Use Word's **Find & Replace (Ctrl+H)** and search for `[INSERT` — it must return zero results when you are done.

### 11.4 Write Section 8 (Cross-Task Synthesis)

Verify the three-layer summary table matches your real results:
- Task 1 row: "Throughput ∝ 1/RTT and ≈1/√p" — confirmed by Tables 5.1/5.2?
- Task 2 row: "PDR drops with each new sender" — confirmed by Table 6.1?
- Task 3 row: "Cubic more aggressive, recovers faster" — confirmed by Table 7.1?

Update any text that contradicts the real numbers.

### 11.5 Fill Peer Evaluation (Section 10)

Fill in real names and adjust contribution percentages if the workload was uneven. Everyone signs.

### 11.6 Assemble submission package

```bash
mkdir -p ~/Group4_Submission/source_code
mkdir -p ~/Group4_Submission/trace_outputs
mkdir -p ~/Group4_Submission/plots
mkdir -p ~/Group4_Submission/screenshots

cp task1_p2p.cc task2_csma.cc task3_dumbbell.cc  ~/Group4_Submission/source_code/
cp task1_results.csv task2_csma.xml               ~/Group4_Submission/trace_outputs/
cp cwnd-newreno.dat cwnd-cubic.dat                ~/Group4_Submission/trace_outputs/
cp figure_5_2.png figure_6_2.png cwnd_comparison.png ~/Group4_Submission/plots/
cp 5.1_task1_run.png 6.1_netanim.png \
   6.2_task2_terminal.png 7.1_task3_run.png       ~/Group4_Submission/screenshots/
cp TNN4113_Project_Report.docx                    ~/Group4_Submission/
cp TNN4113_Project_Report.pdf                     ~/Group4_Submission/

cd ~
zip -r Group4_TNN4113_Submission.zip Group4_Submission/
```

---

## 12. Phase 6 — Final Review & Submission

### 12.1 Group review meeting (Day 5 morning)

Video call, screen-share the PDF:
- Student 1 reads Sections 6, 7, 8
- Student 2 reads Sections 5, 7, 8
- Student 3 reads Sections 5, 6, 8
- Student 4 moderates and applies fixes live

### 12.2 Upload to eLeap

1. Log in to eLeap
2. Navigate to TNN4113 project submission
3. Upload `Group4_TNN4113_Submission.zip`
4. Confirm upload success
5. Screenshot the confirmation page

**Do this before 5 PM on 21 May — never at the last minute.**

---

## 13. Troubleshooting

**`E: Unable to locate package sqlite`**
Remove `sqlite` from the apt command. Use `sqlite3` and `libsqlite3-dev` instead — both are already in the corrected command in Section 4.4.

**`Note, selecting 'libgcrypt20-dev' instead of 'libgcrypt-dev'`**
This is not an error. Ubuntu 22.04 renamed the package. The corrected command in Section 4.4 uses `libgcrypt20-dev` directly.

**`./ns3 run hello-simulator` gives an error**
Re-run configure:
```bash
./ns3 clean
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

**`cwnd-newreno.dat` is empty after Task 3 run**
The tracer hook fires after socket creation. Push the schedule to `Seconds(1.5)`:
```cpp
Simulator::Schedule(Seconds(1.5), [&]() { ... });
```

**NetAnim window does not open on WSL2**
Run `wsl --update` in PowerShell, then reopen Ubuntu and try again.

**Both cWnd curves in Task 3 look identical**
The `Config::Set` per-node TCP variant assignment failed silently. Print the node IDs to confirm:
```cpp
std::cout << "Left 0 ID = " << db.GetLeft(0)->GetId() << "\n";
std::cout << "Left 1 ID = " << db.GetLeft(1)->GetId() << "\n";
```
Then verify those IDs match what you pass to `Config::Set`.

**Task 1 sweep script hangs on one combination**
Abort with Ctrl+C, run that single combination manually to see the error message:
```bash
./ns3 run "scratch/task1_p2p --delay=60ms --errRate=0.05"
```

---

## 14. Submission Checklist

### Source code
- [ ] `task1_p2p.cc` — compiles on NS-3.40, runs cleanly
- [ ] `task2_csma.cc` — compiles on NS-3.40, runs cleanly
- [ ] `task3_dumbbell.cc` — compiles on NS-3.40, runs cleanly
- [ ] All files have header comment with task number and student name

### Data outputs
- [ ] `task1_results.csv` — 20 rows of real simulation data
- [ ] `task2_csma.xml` — generated from 5-sender run, opens in NetAnim
- [ ] `cwnd-newreno.dat` — non-empty, two columns
- [ ] `cwnd-cubic.dat` — non-empty, two columns

### Plots (generated, not screenshots)
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
- [ ] Environment table (Section 4.1) filled with exact spec from this guide
- [ ] Zero remaining `[INSERT]` cells in any table
- [ ] Zero remaining `>>> PLACEHOLDER <<<` blocks
- [ ] All 4 names + matric IDs on cover page and Declaration
- [ ] Peer Evaluation signed by all 4 members
- [ ] PDF exported

### Submission
- [ ] ZIP uploaded to eLeap before 21 May 2026 deadline
- [ ] Confirmation screenshot saved
- [ ] All members have a personal backup copy

---

## Quick Command Reference

```bash
# Everyone — verify NS-3.40
cd ~/ns-allinone-3.40/ns-3.40 && ./ns3 --version

# Student 1 — single test run
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.00"

# Student 1 — full 20-combo sweep
./task1_sweep.sh

# Student 2 — all three CSMA scenarios
for n in 1 3 5; do
  ./ns3 run "scratch/task2_csma --nSenders=$n" | tee task2_n${n}.log
done

# Student 2 — open NetAnim
NetAnim

# Student 3 — run dumbbell + plot
./ns3 run scratch/task3_dumbbell && python3 plot_cwnd.py

# Student 4 — assemble zip
cd ~ && zip -r Group4_TNN4113_Submission.zip Group4_Submission/
```
