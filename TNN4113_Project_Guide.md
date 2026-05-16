# TNN4113 Computer Networks — Complete Project Execution Guide

**Project:** Network Performance Analysis Using NS-3 Simulator
**Submission Date:** 21 May 2026
**Team Size:** 4 students
**ns-3 Version Target:** 3.40
**Host OS Target:** Ubuntu 22.04 LTS (native) or Windows 11 + WSL2 + Ubuntu 22.04

---

## Table of Contents

1. [Team Roles & Responsibilities](#1-team-roles--responsibilities)
2. [Recommended Timeline (5 Days)](#2-recommended-timeline-5-days)
3. [Phase 0 — Shared Setup (Day 1, Everyone)](#3-phase-0--shared-setup-day-1-everyone)
4. [Phase 1 — Install NS-3.40 (Day 1, Everyone)](#4-phase-1--install-ns-340-day-1-everyone)
5. [Phase 2 — Install NetAnim (Day 1, Student 2 mandatory, others optional)](#5-phase-2--install-netanim-day-1-student-2-mandatory-others-optional)
6. [Phase 3 — Set Up Shared Git Repo (Day 1, Student 4 leads)](#6-phase-3--set-up-shared-git-repo-day-1-student-4-leads)
7. [Phase 4a — Task 1: Point-to-Point (Student 1)](#7-phase-4a--task-1-point-to-point-student-1)
8. [Phase 4b — Task 2: CSMA LAN (Student 2)](#8-phase-4b--task-2-csma-lan-student-2)
9. [Phase 4c — Task 3: Dumbbell + TCP (Student 3)](#9-phase-4c--task-3-dumbbell--tcp-student-3)
10. [Phase 5 — Synthesis & Report Assembly (Student 4)](#10-phase-5--synthesis--report-assembly-student-4)
11. [Phase 6 — Final Review & Submission (Whole Team)](#11-phase-6--final-review--submission-whole-team)
12. [Troubleshooting](#12-troubleshooting)
13. [Submission Checklist](#13-submission-checklist)

---

## 1. Team Roles & Responsibilities

| Student | Role | Primary Deliverables |
|---|---|---|
| **Student 1** | Task 1 owner — Point-to-Point | `task1_p2p.cc`, `task1_results.csv`, Tables 5.1 & 5.2, Figure 5.2, Section 5 of report |
| **Student 2** | Task 2 owner — CSMA LAN | `task2_csma.cc`, `task2_csma.xml`, NetAnim screenshot, Tables 6.1 & 6.2, Figure 6.2, Section 6 of report |
| **Student 3** | Task 3 owner — Dumbbell / TCP | `task3_dumbbell.cc`, `cwnd-newreno.dat`, `cwnd-cubic.dat`, `cwnd_comparison.png`, Table 7.1, Section 7 of report |
| **Student 4** | Task 4 owner — Synthesis & Reporting | Integration of all parts, Sections 1–4, 8, 9, 10, 11, 12; final proofreading; submission package |

**Rule of thumb:** Every student installs ns-3.40 on their own machine even if they're not the task owner — you'll need it for testing and integration.

---

## 2. Recommended Timeline (5 Days)

| Day | Date | Everyone | Student 1 | Student 2 | Student 3 | Student 4 |
|---|---|---|---|---|---|---|
| **Day 1 — Sat 17 May** | Setup | Install Ubuntu/WSL2, ns-3.40, NetAnim, agree on Git repo | Get task1_p2p.cc compiling | Get task2_csma.cc compiling | Get task3_dumbbell.cc compiling | Set up repo, create branches, draft Section 1–4 |
| **Day 2 — Sun 18 May** | Build | Each task owner runs full simulation sweep | Run 20 parameter combos, fill Tables 5.1/5.2 | Run 1/3/5 sender scenarios, capture NetAnim | Run Dumbbell, generate .dat files, plot cwnd | Continue Sections 1–4 |
| **Day 3 — Mon 19 May** | Analyze | Each task owner writes their analysis section | Write Section 5 analysis, finalize Figure 5.2 | Write Section 6 analysis, take screenshots | Identify drop events, fill Table 7.1, write Section 7 | Draft Section 8 (Synthesis) |
| **Day 4 — Tue 20 May** | Integrate | Student 4 merges everything | Hand off to S4 | Hand off to S4 | Hand off to S4 | Replace ALL placeholders, write Sections 8/9, polish |
| **Day 5 — Wed 21 May** | Submit | Final review + submission before deadline | Review S4's merged Section 5 | Review S4's merged Section 6 | Review S4's merged Section 7 | Final formatting, generate PDF, upload to eLeap |

---

## 3. Phase 0 — Shared Setup (Day 1, Everyone)

### 3.1 If you're on Windows 11 — Install WSL2 + Ubuntu 22.04

Open **PowerShell as Administrator** and run:

```powershell
wsl --install -d Ubuntu-22.04
```

Reboot if prompted. After reboot, open Ubuntu from the Start menu, set a UNIX username + password.

Update WSL kernel (PowerShell):
```powershell
wsl --update
```

### 3.2 If you're on native Ubuntu 22.04 — Skip to 3.3

### 3.3 Install required system packages

Inside your Ubuntu shell (WSL or native), run:

```bash
sudo apt update && sudo apt upgrade -y
```

Then install all dependencies ns-3.40 needs:

```bash
sudo apt install -y \
  build-essential cmake ninja-build pkg-config \
  g++ gdb gcc-multilib g++-multilib \
  python3 python3-dev python3-pip python3-setuptools \
  git mercurial \
  qtbase5-dev qttools5-dev qttools5-dev-tools qtchooser qt5-qmake \
  mpi-default-bin mpi-default-dev openmpi-bin openmpi-common openmpi-doc libopenmpi-dev \
  autoconf automake libxml2 libxml2-dev libgcrypt-dev libgsl-dev \
  flex bison libfl-dev tcpdump sqlite sqlite3 libsqlite3-dev \
  libgtk-3-dev gnuplot wget tar bzip2 unzip
```

Then install matplotlib for plotting:

```bash
pip3 install --user matplotlib numpy
```

### 3.4 Verify your toolchain

```bash
g++ --version          # should be 11.x or newer
python3 --version      # should be 3.10 or newer
cmake --version        # should be 3.22 or newer
qmake --version        # should report Qt 5.15.x
gnuplot --version      # should be 5.4
```

If any are missing, fix that before going further.

---

## 4. Phase 1 — Install NS-3.40 (Day 1, Everyone)

You have two options. **Pick Option A unless you hit a network issue.**

### 4.1 Option A — Official tarball (recommended)

```bash
cd ~
wget https://www.nsnam.org/releases/ns-allinone-3.40.tar.bz2
tar xjf ns-allinone-3.40.tar.bz2
cd ns-allinone-3.40
./build.py --enable-examples --enable-tests
```

This downloads ≈70 MB and compiles for 15–40 minutes depending on CPU. Be patient.

When it finishes you should see something like:

```
Build finished successfully.
```

Your working ns-3 root will be:

```
~/ns-allinone-3.40/ns-3.40/
```

### 4.2 Option B — Git clone (use if tarball download fails)

```bash
cd ~
git clone https://gitlab.com/nsnam/ns-3-dev.git ns-3.40
cd ns-3.40
git checkout ns-3.40
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

Your working ns-3 root will be:

```
~/ns-3.40/
```

### 4.3 Verify NS-3 works

From your ns-3 root directory:

```bash
cd ~/ns-allinone-3.40/ns-3.40   # or ~/ns-3.40 if Option B
./ns3 run hello-simulator
```

Expected output:

```
Hello Simulator
```

If you see that, **ns-3 is working**. If not, go to [Troubleshooting](#12-troubleshooting).

### 4.4 Test a real example

```bash
./ns3 run "first"
```

This runs the canonical first.cc example. You should see TCP packet exchange logs. If that works, every example in the ns-3 distribution works for you too.

---

## 5. Phase 2 — Install NetAnim (Day 1, Student 2 mandatory, others optional)

NetAnim is the GUI animator. It's bundled with the ns-allinone tarball but needs to be built separately.

### 5.1 Build NetAnim

```bash
cd ~/ns-allinone-3.40/netanim-3.108
make clean
qmake NetAnim.pro
make -j$(nproc)
```

If the build succeeds you'll see a `NetAnim` executable.

### 5.2 Launch NetAnim (graphical)

**On native Ubuntu:**
```bash
./NetAnim
```

**On WSL2 + Windows 11:** Windows 11 supports Linux GUI apps out of the box (WSLg). Just run:
```bash
./NetAnim
```
A window should pop up. If nothing appears, run:
```bash
sudo apt install -y x11-apps && xeyes
```
to verify GUI forwarding is working.

**On WSL1 or older Windows 10:** install VcXsrv on Windows, set `export DISPLAY=:0` in your shell, then launch.

### 5.3 Add NetAnim to your PATH (optional convenience)

```bash
echo 'export PATH="$HOME/ns-allinone-3.40/netanim-3.108:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Now `NetAnim` works from anywhere.

---

## 6. Phase 3 — Set Up Shared Git Repo (Day 1, Student 4 leads)

Coordination saves you on Day 4. Use GitHub (free, private repos for students).

### 6.1 Student 4 creates the repo

1. Go to https://github.com/new
2. Repo name: `TNN4113-NS3-Project`
3. Visibility: **Private**
4. Initialize with a README
5. Click Create

### 6.2 Student 4 adds teammates as collaborators

Settings → Collaborators → Add people → enter each teammate's GitHub username.

### 6.3 Everyone clones the repo locally

```bash
cd ~
git clone https://github.com/<student4-username>/TNN4113-NS3-Project.git
cd TNN4113-NS3-Project
```

### 6.4 Recommended folder structure

Student 4 creates this structure and pushes:

```
TNN4113-NS3-Project/
├── README.md
├── src/
│   ├── task1_p2p.cc
│   ├── task2_csma.cc
│   └── task3_dumbbell.cc
├── scripts/
│   ├── task1_sweep.sh
│   ├── plot_cwnd.gp
│   └── plot_cwnd.py
├── results/
│   ├── task1_results.csv
│   ├── task2_csma.xml
│   ├── cwnd-newreno.dat
│   ├── cwnd-cubic.dat
│   └── cwnd_comparison.png
├── screenshots/
│   ├── 5.1_task1_run.png
│   ├── 6.1_netanim.png
│   ├── 6.2_task2_terminal.png
│   └── 7.1_task3_run.png
└── report/
    └── TNN4113_Project_Report.docx
```

### 6.5 Branching strategy

```bash
# Student 1
git checkout -b student1-task1

# Student 2
git checkout -b student2-task2

# Student 3
git checkout -b student3-task3

# Student 4 works on main
```

Each student pushes their branch, opens a Pull Request when their task is done. Student 4 merges all PRs into `main` on Day 4.

---

## 7. Phase 4a — Task 1: Point-to-Point (Student 1)

**Goal:** measure how channel delay and packet error rate degrade TCP throughput.

### 7.1 Place the source file

Create `task1_p2p.cc` inside the ns-3 `scratch/` directory:

```bash
cd ~/ns-allinone-3.40/ns-3.40/scratch
nano task1_p2p.cc
```

Paste the **complete, fully commented** source below:

```cpp
/*
 * TNN4113 - Task 1
 * Point-to-Point Network: Link Characteristic & Reliability Analysis
 *
 * Topology:  Node 0  <-- 5 Mbps, variable delay, variable error -->  Node 1
 *            (BulkSendApplication / TCP source)              (PacketSink / TCP sink)
 *
 * Varied:    channel delay  ∈ {2ms, 10ms, 30ms, 60ms, 100ms}
 *            error rate     ∈ {0.00, 0.01, 0.03, 0.05}  (per-packet, on receiver side)
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
    // --------- Default parameter values (overridable via command line) ---------
    std::string delayStr = "10ms";   // varied: 2ms, 10ms, 30ms, 60ms, 100ms
    double      errRate  = 0.00;     // varied: 0.00, 0.01, 0.03, 0.05
    uint32_t    simTime  = 30;       // seconds

    CommandLine cmd;
    cmd.AddValue("delay",   "Channel propagation delay",        delayStr);
    cmd.AddValue("errRate", "Per-packet error rate at receiver", errRate);
    cmd.AddValue("simTime", "Simulation time in seconds",        simTime);
    cmd.Parse(argc, argv);

    // --------- Force TCP variant to NewReno (Task 1 spec) ---------
    Config::SetDefault("ns3::TcpL4Protocol::SocketType",
                       StringValue("ns3::TcpNewReno"));

    // --------- Topology: 2 nodes, 1 p2p link ---------
    NodeContainer nodes;
    nodes.Create(2);

    PointToPointHelper p2p;
    p2p.SetDeviceAttribute("DataRate", StringValue("5Mbps"));
    p2p.SetChannelAttribute("Delay",   StringValue(delayStr));

    NetDeviceContainer devs = p2p.Install(nodes);

    // --------- Inject packet loss at the receiver NIC ---------
    Ptr<RateErrorModel> em = CreateObject<RateErrorModel>();
    em->SetAttribute("ErrorRate", DoubleValue(errRate));
    em->SetAttribute("ErrorUnit",
                     EnumValue(RateErrorModel::ERROR_UNIT_PACKET));
    devs.Get(1)->SetAttribute("ReceiveErrorModel", PointerValue(em));

    // --------- Internet stack and IPv4 addressing ---------
    InternetStackHelper stack;
    stack.Install(nodes);

    Ipv4AddressHelper addr;
    addr.SetBase("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer ifs = addr.Assign(devs);

    // --------- BulkSend on Node 0, PacketSink on Node 1 ---------
    uint16_t port = 9;
    BulkSendHelper src("ns3::TcpSocketFactory",
                       InetSocketAddress(ifs.GetAddress(1), port));
    src.SetAttribute("MaxBytes", UintegerValue(0));   // 0 = unbounded
    ApplicationContainer srcApp = src.Install(nodes.Get(0));
    srcApp.Start(Seconds(1.0));
    srcApp.Stop(Seconds(simTime));

    PacketSinkHelper sink("ns3::TcpSocketFactory",
                          InetSocketAddress(Ipv4Address::GetAny(), port));
    ApplicationContainer sinkApp = sink.Install(nodes.Get(1));
    sinkApp.Start(Seconds(0.0));
    sinkApp.Stop(Seconds(simTime));

    // --------- FlowMonitor on all nodes ---------
    FlowMonitorHelper fmh;
    Ptr<FlowMonitor> fm = fmh.InstallAll();

    Simulator::Stop(Seconds(simTime));
    Simulator::Run();

    fm->CheckForLostPackets();
    auto stats = fm->GetFlowStats();

    // --------- Print result line for each flow ---------
    for (auto &kv : stats) {
        double thrMbps = kv.second.rxBytes * 8.0 / ((simTime - 1) * 1e6);
        double delayMs = 0.0;
        if (kv.second.rxPackets > 0) {
            delayMs = (kv.second.delaySum.GetSeconds() / kv.second.rxPackets) * 1000.0;
        }
        uint64_t lost = kv.second.lostPackets;
        std::cout << "Flow "        << kv.first
                  << "  Throughput=" << thrMbps << " Mbps"
                  << "  MeanDelay="  << delayMs << " ms"
                  << "  Lost="       << lost
                  << "  TxPackets="  << kv.second.txPackets
                  << "  RxPackets="  << kv.second.rxPackets
                  << std::endl;
    }

    Simulator::Destroy();
    return 0;
}
```

Save and exit nano (`Ctrl+O`, Enter, `Ctrl+X`).

### 7.2 Compile and do a single test run

```bash
cd ~/ns-allinone-3.40/ns-3.40
./ns3 build
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.01"
```

Expected output is one line like:
```
Flow 1  Throughput=3.X Mbps  MeanDelay=2X ms  Lost=N  TxPackets=N  RxPackets=N
```

If you got a number, **Task 1 is operational.**

### 7.3 The 20-combination sweep

Create the sweep script. Inside the ns-3 root:

```bash
nano task1_sweep.sh
```

Paste:

```bash
#!/bin/bash
# task1_sweep.sh — iterate every (delay, error-rate) combination
# Output: task1_results.csv with columns delay,errRate,throughput,meanDelay,lost,pdr

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
echo "Done. Results in $OUT"
column -s, -t "$OUT"
```

Make executable and run:

```bash
chmod +x task1_sweep.sh
./task1_sweep.sh
```

Total runtime: ≈10–20 minutes for all 20 combinations. When done, `task1_results.csv` contains every data point you need for Tables 5.1 and 5.2.

### 7.4 Capture Screenshot 5.1

While one of the runs is still on your terminal, take a screenshot showing:
- the `./ns3 build` line succeeding
- the `./ns3 run "scratch/task1_p2p ..."` line
- the `Flow 1 Throughput=...` output line

Save it to `screenshots/5.1_task1_run.png`.

### 7.5 Plot Figure 5.2 — Throughput vs Delay (4 error-rate curves)

Create `plot_task1.py` in the ns-3 root:

```python
#!/usr/bin/env python3
"""plot_task1.py — Figure 5.2: TCP throughput vs channel delay and error rate."""
import csv
import matplotlib.pyplot as plt
from collections import defaultdict

data = defaultdict(list)  # data[errRate] = [(delay, thr), ...]
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

Run it:

```bash
python3 plot_task1.py
```

Output: `figure_5_2.png` — drop this into your report at Figure 5.2.

### 7.6 Fill in the report tables

Open `task1_results.csv` in a spreadsheet. Pick the rows you need:

**Table 5.1** (Throughput vs Delay, error rate fixed at 0.00):
| delay_ms | throughput | meanDelay | lost |
|---|---|---|---|
| 2 | (your row) | (your row) | (your row) |
| 10 | … | … | … |
| 30 | … | … | … |
| 60 | … | … | … |
| 100 | … | … | … |

**Table 5.2** (Throughput vs Error Rate, delay fixed at 10 ms):
| errRate | throughput | tx | rx | pdr_pct |
|---|---|---|---|---|
| 0.00 | … | … | … | … |
| 0.01 | … | … | … | … |
| 0.03 | … | … | … | … |
| 0.05 | … | … | … | … |

Copy these into Word, replacing all `[INSERT]` cells in Sections 5.5 and 5.6.

### 7.7 Cross-check analysis text (Section 5.8)

Before declaring done, look at your numbers:
- Did throughput **fall** monotonically as delay rose? It should.
- Did throughput **collapse** as error rate rose? It should.
- If yes, the pre-written Section 5.8 prose is consistent with your data → leave it.
- If your numbers disagree (e.g. throughput rose with delay — would mean a bug), don't write fiction; debug your script.

### 7.8 Commit your work

```bash
cp scratch/task1_p2p.cc ~/TNN4113-NS3-Project/src/
cp task1_sweep.sh        ~/TNN4113-NS3-Project/scripts/
cp task1_results.csv     ~/TNN4113-NS3-Project/results/
cp figure_5_2.png        ~/TNN4113-NS3-Project/results/
cp plot_task1.py         ~/TNN4113-NS3-Project/scripts/
cp screenshots/5.1*.png  ~/TNN4113-NS3-Project/screenshots/

cd ~/TNN4113-NS3-Project
git checkout student1-task1
git add .
git commit -m "Student 1: complete Task 1 simulation + plots"
git push origin student1-task1
```

Open a Pull Request on GitHub targeting `main`. Student 4 will merge.

---

## 8. Phase 4b — Task 2: CSMA LAN (Student 2)

**Goal:** measure how throughput and PDR degrade as more senders contend on a shared CSMA medium.

### 8.1 Place the source file

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
 *            N0 = sink (PacketSink)
 *            N1..NnSenders = OnOff/UDP senders, each at 5 Mbps CBR
 *
 * Varied:    nSenders ∈ {1, 3, 5}
 *
 * Measured:  per-flow throughput, per-flow PDR,
 *            NetAnim XML for visualization.
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
    uint32_t nSenders = 1;       // 1, 3, or 5
    uint32_t simTime  = 20;      // seconds

    CommandLine cmd;
    cmd.AddValue("nSenders", "Number of active senders (1, 3, or 5)", nSenders);
    cmd.AddValue("simTime",  "Simulation time in seconds",             simTime);
    cmd.Parse(argc, argv);

    // --------- Topology: 10-node CSMA LAN ---------
    NodeContainer lan;
    lan.Create(10);

    CsmaHelper csma;
    csma.SetChannelAttribute("DataRate", StringValue("100Mbps"));
    csma.SetChannelAttribute("Delay",    TimeValue(NanoSeconds(6560)));

    NetDeviceContainer devs = csma.Install(lan);

    // --------- Internet stack and IPv4 addressing ---------
    InternetStackHelper stack;
    stack.Install(lan);

    Ipv4AddressHelper addr;
    addr.SetBase("10.2.1.0", "255.255.255.0");
    Ipv4InterfaceContainer ifs = addr.Assign(devs);

    // --------- Sink on N0 ---------
    uint16_t port = 7;
    PacketSinkHelper sink("ns3::UdpSocketFactory",
                          InetSocketAddress(Ipv4Address::GetAny(), port));
    ApplicationContainer sinkApp = sink.Install(lan.Get(0));
    sinkApp.Start(Seconds(0.0));
    sinkApp.Stop(Seconds(simTime));

    // --------- Senders N1..NnSenders, all 5 Mbps CBR UDP ---------
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
        a.Start(Seconds(1.0 + 0.01 * i));    // stagger by 10ms each
        a.Stop(Seconds(simTime));
    }

    // --------- NetAnim XML for visualisation ---------
    AnimationInterface anim("task2_csma.xml");
    for (uint32_t i = 0; i < lan.GetN(); ++i) {
        anim.SetConstantPosition(lan.Get(i), i * 5.0, 10.0);
        anim.UpdateNodeDescription(lan.Get(i),
            (i == 0) ? "SINK-N0" : ("N" + std::to_string(i)));
    }

    // --------- FlowMonitor on all nodes ---------
    FlowMonitorHelper fmh;
    Ptr<FlowMonitor> fm = fmh.InstallAll();

    Simulator::Stop(Seconds(simTime));
    Simulator::Run();

    fm->CheckForLostPackets();
    auto cs = DynamicCast<Ipv4FlowClassifier>(fmh.GetClassifier());
    auto stats = fm->GetFlowStats();

    std::cout << "\n=== Task 2 results (nSenders=" << nSenders << ") ===\n";
    double aggThr = 0.0;
    int    nFlows = 0;
    for (auto &kv : stats) {
        auto t = cs->FindFlow(kv.first);
        double thrMbps = kv.second.rxBytes * 8.0 / ((simTime - 1) * 1e6);
        double pdr = (kv.second.txPackets == 0) ? 0.0 :
                     100.0 * kv.second.rxPackets / kv.second.txPackets;

        std::cout << "Flow " << kv.first
                  << "  " << t.sourceAddress << " -> " << t.destinationAddress
                  << "  Thr=" << thrMbps << " Mbps"
                  << "  Tx="  << kv.second.txPackets
                  << "  Rx="  << kv.second.rxPackets
                  << "  PDR=" << pdr << "%\n";
        aggThr += thrMbps;
        nFlows++;
    }
    std::cout << "AGGREGATE Throughput at sink = " << aggThr
              << " Mbps  (across " << nFlows << " flows)\n";

    Simulator::Destroy();
    return 0;
}
```

Save (`Ctrl+O`, Enter, `Ctrl+X`).

### 8.2 Build and test

```bash
cd ~/ns-allinone-3.40/ns-3.40
./ns3 build
./ns3 run "scratch/task2_csma --nSenders=1"
```

You should see one flow line and an `AGGREGATE Throughput` line.

### 8.3 Run all three scenarios

```bash
./ns3 run "scratch/task2_csma --nSenders=1"  | tee task2_n1.log
./ns3 run "scratch/task2_csma --nSenders=3"  | tee task2_n3.log
./ns3 run "scratch/task2_csma --nSenders=5"  | tee task2_n5.log
```

Each run regenerates `task2_csma.xml`. **Important:** the XML you keep for submission should come from the **5-sender run** (it's the richest).

After the 5-sender run finishes, save the XML:
```bash
cp task2_csma.xml task2_csma_5senders.xml
```

### 8.4 Open NetAnim and capture Screenshot 6.1

Launch NetAnim:

```bash
~/ns-allinone-3.40/netanim-3.108/NetAnim
```

1. Click the **folder icon** (top-left) → open `~/ns-allinone-3.40/ns-3.40/task2_csma.xml`
2. You should see 10 nodes laid out in a horizontal row, N0 labelled "SINK-N0"
3. Click the **green Play button**
4. Frame-exchange arrows will appear between sender nodes and the sink
5. Pause when animation is active — take a screenshot showing:
   - All 10 nodes visible
   - Animation in progress (you'll see directed arrows)
6. Save as `screenshots/6.1_netanim.png`

### 8.5 Capture Screenshot 6.2 — Terminal FlowMonitor output

From `task2_n5.log`, take a screenshot of the terminal showing the 5-flow output + aggregate throughput. Save as `screenshots/6.2_task2_terminal.png`.

### 8.6 Plot Figure 6.2 — Aggregate throughput + PDR vs senders

Quick way: open a Python REPL with your three log values:

```bash
python3 << 'EOF'
import matplotlib.pyplot as plt
import numpy as np

# Fill these in from your task2_n1.log / n3.log / n5.log AGGREGATE & PDR lines
senders     = [1, 3, 5]
agg_thr     = [4.99, 14.85, 24.00]   # REPLACE with your real values
mean_pdr    = [100.0, 99.0, 96.0]    # REPLACE with your real values

fig, ax1 = plt.subplots(figsize=(8, 5))
x = np.arange(len(senders))
width = 0.35

ax1.bar(x - width/2, agg_thr, width, color='steelblue', label='Aggregate Throughput (Mbps)')
ax1.set_xlabel('Number of Active Senders')
ax1.set_ylabel('Aggregate Throughput (Mbps)', color='steelblue')
ax1.tick_params(axis='y', labelcolor='steelblue')
ax1.set_xticks(x)
ax1.set_xticklabels(senders)

ax2 = ax1.twinx()
ax2.bar(x + width/2, mean_pdr, width, color='indianred', label='Mean PDR (%)')
ax2.set_ylabel('Mean PDR (%)', color='indianred')
ax2.tick_params(axis='y', labelcolor='indianred')
ax2.set_ylim(0, 105)

plt.title('Aggregate Throughput and PDR vs Number of CSMA Senders')
fig.tight_layout()
plt.savefig('figure_6_2.png', dpi=150)
print('Saved figure_6_2.png')
EOF
```

### 8.7 Fill in the report tables

From your three log files, populate Table 6.1 (`nSenders=1/3/5` summary) and Table 6.2 (per-flow detail for `nSenders=5`).

### 8.8 Commit

```bash
cp scratch/task2_csma.cc       ~/TNN4113-NS3-Project/src/
cp task2_csma_5senders.xml     ~/TNN4113-NS3-Project/results/task2_csma.xml
cp figure_6_2.png              ~/TNN4113-NS3-Project/results/
cp task2_n*.log                ~/TNN4113-NS3-Project/results/
cp screenshots/6.*.png         ~/TNN4113-NS3-Project/screenshots/

cd ~/TNN4113-NS3-Project
git checkout student2-task2
git add .
git commit -m "Student 2: complete Task 2 simulation + NetAnim + plots"
git push origin student2-task2
```

Open PR → Student 4 merges.

---

## 9. Phase 4c — Task 3: Dumbbell + TCP (Student 3)

**Goal:** compare TCP NewReno vs TCP Cubic on a shared bottleneck; plot the cWnd evolution.

### 9.1 Place the source file

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
 *      L0 ----\                      /---- R0
 *              left-router --- bottleneck --- right-router
 *      L1 ----/    1 Mbps, 20ms, DropTail-50p           \---- R1
 *
 *   L0 -> R0 : TCP NewReno (BulkSend)
 *   L1 -> R1 : TCP Cubic   (BulkSend)
 *
 *   cWnd of each sender is traced to .dat files every time it changes.
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

    // --------- Link templates ---------
    PointToPointHelper accessLink;
    accessLink.SetDeviceAttribute("DataRate", StringValue("10Mbps"));
    accessLink.SetChannelAttribute("Delay",   StringValue("1ms"));

    PointToPointHelper bottleneck;
    bottleneck.SetDeviceAttribute("DataRate", StringValue("1Mbps"));
    bottleneck.SetChannelAttribute("Delay",   StringValue("20ms"));
    bottleneck.SetQueue("ns3::DropTailQueue",
                        "MaxSize", StringValue("50p"));

    // --------- Build dumbbell: 2 left, 2 right ---------
    PointToPointDumbbellHelper db(2, accessLink,
                                  2, accessLink,
                                  bottleneck);

    InternetStackHelper stack;
    db.InstallStack(stack);

    db.AssignIpv4Addresses(
        Ipv4AddressHelper("10.1.0.0", "255.255.255.0"),
        Ipv4AddressHelper("10.2.0.0", "255.255.255.0"),
        Ipv4AddressHelper("10.3.0.0", "255.255.255.0"));

    // --------- Per-node TCP variant: L0 = NewReno, L1 = Cubic ---------
    TypeId tidReno  = TypeId::LookupByName("ns3::TcpNewReno");
    TypeId tidCubic = TypeId::LookupByName("ns3::TcpCubic");

    Config::Set("/NodeList/" + std::to_string(db.GetLeft(0)->GetId()) +
                "/$ns3::TcpL4Protocol/SocketType", TypeIdValue(tidReno));
    Config::Set("/NodeList/" + std::to_string(db.GetLeft(1)->GetId()) +
                "/$ns3::TcpL4Protocol/SocketType", TypeIdValue(tidCubic));

    uint16_t port = 5001;

    // --------- Sinks on right side ---------
    for (uint32_t i = 0; i < 2; ++i) {
        PacketSinkHelper sink("ns3::TcpSocketFactory",
            InetSocketAddress(Ipv4Address::GetAny(), port));
        ApplicationContainer s = sink.Install(db.GetRight(i));
        s.Start(Seconds(0.0));
        s.Stop(Seconds(simTime));
    }

    // --------- Sources on left side ---------
    for (uint32_t i = 0; i < 2; ++i) {
        BulkSendHelper src("ns3::TcpSocketFactory",
            InetSocketAddress(db.GetRightIpv4Address(i), port));
        src.SetAttribute("MaxBytes", UintegerValue(0));
        ApplicationContainer a = src.Install(db.GetLeft(i));
        a.Start(Seconds(1.0));
        a.Stop(Seconds(simTime));
    }

    // --------- Connect cWnd tracers AFTER sockets exist ---------
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

    std::cout << "Task 3 finished. Output files:\n"
              << "  cwnd-newreno.dat\n"
              << "  cwnd-cubic.dat\n";
    return 0;
}
```

Save.

### 9.2 Build and run

```bash
cd ~/ns-allinone-3.40/ns-3.40
./ns3 build
./ns3 run scratch/task3_dumbbell
```

After it completes, verify the output files exist:

```bash
ls -l cwnd-newreno.dat cwnd-cubic.dat
head cwnd-newreno.dat
head cwnd-cubic.dat
```

Each should be hundreds-to-thousands of lines of `time\tcwnd_bytes`.

### 9.3 Capture Screenshot 7.1

Take a screenshot of the terminal showing the `./ns3 run` command, the "Task 3 finished" output, and the `ls -l` line confirming both `.dat` files exist. Save as `screenshots/7.1_task3_run.png`.

### 9.4 Plot the cWnd comparison (Figure 7.2)

Create `plot_cwnd.py` in the ns-3 root:

```python
#!/usr/bin/env python3
"""plot_cwnd.py — Figure 7.2: cWnd evolution NewReno vs Cubic."""
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

Run:

```bash
python3 plot_cwnd.py
```

Or use gnuplot:

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

Result: `cwnd_comparison.png` — drop into report as Figure 7.2.

### 9.5 Fill Table 7.1 — Drop & Recovery events

This is the tricky part. You need to identify the **first 3 drop events** for each variant.

A "drop event" = cWnd suddenly decreases. Detect them with this script:

```bash
python3 << 'EOF'
import numpy as np

for label, fn in [('NewReno', 'cwnd-newreno.dat'), ('Cubic', 'cwnd-cubic.dat')]:
    d = np.loadtxt(fn)
    t, cwnd = d[:, 0], d[:, 1]
    drops = []
    for i in range(1, len(cwnd)):
        if cwnd[i] < cwnd[i-1] * 0.95:  # >5% sudden drop = real congestion event
            w_max = cwnd[i-1]
            w_min = cwnd[i]
            t_drop = t[i]
            # Find recovery: time when cWnd next reaches w_max
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
        rt_str = f"{rt:.3f}s" if rt is not None else "N/A (never recovered)"
        print(f"  Drop {k}: t={td:.3f}s  W_max={int(wmx)}  W_min={int(wmn)}  Recovery={rt_str}")
EOF
```

Run it. Output gives you the exact numbers to put in Table 7.1.

### 9.6 Cross-check analysis text (Section 7.7)

Look at your `cwnd_comparison.png`:
- Is NewReno a classic sawtooth (linear climbs)? ✓
- Is Cubic visibly different — flat near W_max, then steep climb? ✓
- Does Cubic reach higher cWnd values between drops? ✓
- Is Cubic's recovery time shorter than NewReno's in Table 7.1? ✓

If all four are yes → the pre-written Section 7.7 prose holds. If any answer is no, debug — most likely cause is sockets being created in a different order than expected (swap NewReno/Cubic node assignments).

### 9.7 Commit

```bash
cp scratch/task3_dumbbell.cc  ~/TNN4113-NS3-Project/src/
cp cwnd-newreno.dat           ~/TNN4113-NS3-Project/results/
cp cwnd-cubic.dat             ~/TNN4113-NS3-Project/results/
cp cwnd_comparison.png        ~/TNN4113-NS3-Project/results/
cp plot_cwnd.py plot_cwnd.gp  ~/TNN4113-NS3-Project/scripts/
cp screenshots/7.1*.png       ~/TNN4113-NS3-Project/screenshots/

cd ~/TNN4113-NS3-Project
git checkout student3-task3
git add .
git commit -m "Student 3: complete Task 3 dumbbell simulation + cWnd analysis"
git push origin student3-task3
```

Open PR → Student 4 merges.

---

## 10. Phase 5 — Synthesis & Report Assembly (Student 4)

You don't do simulation work. You produce the final deliverable.

### 10.1 Pull everything from the other students

```bash
cd ~/TNN4113-NS3-Project
git checkout main
git pull
# Merge each branch via GitHub PR UI, or:
git merge student1-task1
git merge student2-task2
git merge student3-task3
git push origin main
```

After merging you should have:
- `src/` — all 3 .cc files
- `results/` — all CSV, XML, DAT, PNG files
- `screenshots/` — all 4 mandatory screenshots
- `scripts/` — sweep, plotting helpers

### 10.2 Open the report template

Open `TNN4113_Project_Report.docx` in Word (or LibreOffice).

### 10.3 Replace ALL placeholder text — systematic checklist

Search for each marker and replace:

| Find | Replace with |
|---|---|
| `[INSERT NAME]` (8 occurrences) | Real student names |
| `[INSERT ID]` (4 occurrences) | Real matric IDs |
| `>>> PLACEHOLDER: INSERT UNIVERSITY ...<<<` | Paste university logo image |
| `>>> PLACEHOLDER: INSERT FIGURE 5.1 ...<<<` | Insert hand-drawn or Visio P2P diagram |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 5.1 ...<<<` | Insert `screenshots/5.1_task1_run.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 5.2 ...<<<` | Insert `results/figure_5_2.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 6.1 ...<<<` | Insert hand-drawn or Visio CSMA diagram |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 6.1 ...<<<` | Insert `screenshots/6.1_netanim.png` |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 6.2 ...<<<` | Insert `screenshots/6.2_task2_terminal.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 6.2 ...<<<` | Insert `results/figure_6_2.png` |
| `>>> PLACEHOLDER: INSERT FIGURE 7.1 ...<<<` | Insert hand-drawn or Visio Dumbbell diagram |
| `>>> PLACEHOLDER: INSERT FIGURE 7.2 ...<<<` | Insert `results/cwnd_comparison.png` |
| `>>> PLACEHOLDER: INSERT SCREENSHOT 7.1 ...<<<` | Insert `screenshots/7.1_task3_run.png` |
| Every `[INSERT]` cell in Tables 5.1, 5.2, 6.1, 6.2, 7.1 | Real numbers from CSV/logs |
| Every `[INSERT approx X.XX]` hint | Either delete or replace with real number |

**Pro tip:** use Word's Find & Replace (Ctrl+H). Search for `[INSERT` to find every remaining placeholder. After you finish, that search must return zero results.

### 10.4 Sketch the three topology diagrams (Figures 5.1, 6.1, 7.1)

Easiest path: use **draw.io** (free, web-based at https://app.diagrams.net):

- **Figure 5.1 — P2P:** two circles labelled "Node 0 (TCP Source)" and "Node 1 (TCP Sink)", connected by a line labelled "5 Mbps, variable delay, variable error rate"
- **Figure 6.1 — CSMA:** ten circles in a row (N0..N9), all connected to one horizontal line (the shared channel); colour N0 differently and label it "SINK"
- **Figure 7.1 — Dumbbell:** L0, L1 on left → left router → bottleneck (label "1 Mbps, 20ms, DropTail 50p") → right router → R0, R1 on right; label access links "10 Mbps, 1 ms"

Export each as PNG, insert into Word at the corresponding placeholder.

### 10.5 Confirm Section 8 synthesis table values

Table in Section 8.1 references the headline result from each task. Verify the phrasing matches your actual data:
- Task 1 "Throughput falls as 1/RTT and ≈1/√p" — your Tables 5.1 and 5.2 should support this
- Task 2 "PDR drops with each new sender" — your Table 6.1 should show this
- Task 3 "Cubic more aggressive, recovers faster" — your Table 7.1 should confirm this

### 10.6 Fill the Peer Evaluation / Contribution Sheet (Section 10)

The pre-filled 25%/25%/25%/25% split is fine for an evenly-distributed team. Replace `[INSERT NAME 1]`...`[INSERT NAME 4]` with real names and update specific contributions if anyone went above/beyond.

Each member signs (digital signature image, or print → sign → scan).

### 10.7 Generate the final PDF

In Word: **File → Export → Create PDF/XPS** → save as `TNN4113_Project_Report.pdf` in your final submission folder.

### 10.8 Assemble the submission package

Create a clean folder:

```bash
mkdir -p ~/Group4_Submission
cd ~/Group4_Submission

# Source code
mkdir source_code
cp ~/TNN4113-NS3-Project/src/*.cc source_code/

# Trace and data outputs
mkdir trace_outputs
cp ~/TNN4113-NS3-Project/results/task1_results.csv trace_outputs/
cp ~/TNN4113-NS3-Project/results/task2_csma.xml trace_outputs/
cp ~/TNN4113-NS3-Project/results/cwnd-newreno.dat trace_outputs/
cp ~/TNN4113-NS3-Project/results/cwnd-cubic.dat trace_outputs/

# Plots
mkdir plots
cp ~/TNN4113-NS3-Project/results/figure_5_2.png plots/
cp ~/TNN4113-NS3-Project/results/figure_6_2.png plots/
cp ~/TNN4113-NS3-Project/results/cwnd_comparison.png plots/

# Screenshots
mkdir screenshots
cp ~/TNN4113-NS3-Project/screenshots/*.png screenshots/

# Report
cp ~/Documents/TNN4113_Project_Report.docx .
cp ~/Documents/TNN4113_Project_Report.pdf .

# Zip it
cd ~
zip -r Group4_TNN4113_Submission.zip Group4_Submission/
```

You now have one zip file ready to upload to eLeap.

---

## 11. Phase 6 — Final Review & Submission (Whole Team)

### 11.1 Cross-review meeting (everyone, evening of Day 5)

Sit on a Discord/Zoom call, screen-share the PDF. Each member proof-reads the other three sections:

- **Student 1** reads Sections 6, 7, 8 — calls out any errors
- **Student 2** reads Sections 5, 7, 8 — calls out any errors
- **Student 3** reads Sections 5, 6, 8 — calls out any errors
- **Student 4** moderates and applies fixes in real-time

### 11.2 Final checks

Use [Submission Checklist](#13-submission-checklist) below.

### 11.3 Upload

1. Log in to eLeap.
2. Navigate to the TNN4113 project submission area.
3. Upload `Group4_TNN4113_Submission.zip`.
4. Confirm upload succeeded.
5. Screenshot the confirmation page for proof.

**Do this before 5 PM on submission day — never on the deadline minute.**

---

## 12. Troubleshooting

### 12.1 `./ns3 run hello-simulator` produces an error

Re-run the configure step:
```bash
./ns3 clean
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

### 12.2 Build error: `error: 'CommandLine' was not declared`

You forgot to include `"ns3/core-module.h"` at the top of your .cc file.

### 12.3 NetAnim won't open the XML

- Make sure the XML file size is non-zero (`ls -l task2_csma.xml`).
- Make sure your NetAnim version matches the bundled one (3.108 with ns-3.40).
- If using WSL: ensure WSLg is enabled (`wsl --update` in PowerShell).

### 12.4 `cwnd-newreno.dat` is empty

The tracer hook fires only after the TCP socket exists. The code uses `Simulator::Schedule(Seconds(1.001), ...)`. If your sources start at `Seconds(1.0)`, the socket may not exist yet at exactly 1.001s on slow machines. Push the schedule to `Seconds(1.5)` or push source start to `Seconds(0.5)` to widen the gap.

### 12.5 Task 1 sweep script never finishes

The script runs 20 simulations sequentially, each ≈30 s of simulated time but only seconds of wall-clock time. If it's hanging on one combination, abort with Ctrl+C, run that single combination by hand to see the error.

### 12.6 Throughput numbers look identical for different runs

You're not changing the RNG seed. For repeatability checks, append `--RngRun=2` (then 3, 4, 5) to a sweep variant.

### 12.7 NetAnim shows nodes overlapping

You forgot `anim.SetConstantPosition(...)` for each node, or you set them all to the same coordinate. The code above lays them out in a horizontal row spaced 5 metres apart.

### 12.8 Cubic and NewReno curves look the same

Verify your per-node `Config::Set` line ran *before* the sockets were created. The pattern `/NodeList/<id>/$ns3::TcpL4Protocol/SocketType` must use the dollar-sign aggregate form. Also verify ns-3.40 has TcpCubic — list with:
```bash
grep -r "TcpCubic" src/internet/model/ | head -5
```

### 12.9 Git push asks for password (GitHub deprecated passwords)

Use a Personal Access Token instead:
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token (classic)
2. Scope: `repo`
3. Copy the token, use it as your password when prompted

Or set up SSH keys (`ssh-keygen -t ed25519` → add to GitHub).

---

## 13. Submission Checklist

Before uploading, every box must be ticked.

### Source code
- [ ] `task1_p2p.cc` — compiles cleanly on ns-3.40
- [ ] `task2_csma.cc` — compiles cleanly on ns-3.40
- [ ] `task3_dumbbell.cc` — compiles cleanly on ns-3.40
- [ ] All files have a header comment with task number and student name
- [ ] Code is commented (not just stripped down)

### Data outputs
- [ ] `task1_results.csv` exists with 20 rows of real data
- [ ] `task2_csma.xml` exists and opens in NetAnim
- [ ] `cwnd-newreno.dat` and `cwnd-cubic.dat` exist and contain numeric data
- [ ] `figure_5_2.png` — Throughput vs Delay & Error Rate
- [ ] `figure_6_2.png` — CSMA aggregate throughput & PDR
- [ ] `cwnd_comparison.png` — NewReno vs Cubic cWnd

### Screenshots (4 mandatory)
- [ ] Screenshot 5.1 — Task 1 successful run
- [ ] Screenshot 6.1 — NetAnim CSMA visualization
- [ ] Screenshot 6.2 — Task 2 terminal output
- [ ] Screenshot 7.1 — Task 3 successful run

### Report
- [ ] Every `[INSERT]` table cell replaced with a real number
- [ ] Every `>>> PLACEHOLDER <<<` block replaced with a figure or screenshot
- [ ] All four student names + matric IDs filled on cover page
- [ ] All four signatures captured on Declaration of Authorship
- [ ] Peer Evaluation table filled with real names and percentages summing to 100%
- [ ] All four signatures on Peer Evaluation
- [ ] References section preserved (no `[INSERT]` items)
- [ ] Final PDF exported and ZIP package built

### Logistics
- [ ] ZIP uploaded to eLeap before 21 May 2026 deadline
- [ ] Upload confirmation screenshot saved
- [ ] All team members have a personal backup copy of the ZIP

---

## Quick Reference — Single-Command Cheat Sheet

For each student to keep open in a second terminal:

```bash
# Student 1 — Task 1 single run
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.01"

# Student 1 — Task 1 full sweep
./task1_sweep.sh

# Student 2 — Task 2 all three scenarios
for n in 1 3 5; do ./ns3 run "scratch/task2_csma --nSenders=$n" | tee task2_n${n}.log; done

# Student 2 — Open NetAnim
~/ns-allinone-3.40/netanim-3.108/NetAnim

# Student 3 — Task 3 run + plot
./ns3 run scratch/task3_dumbbell && python3 plot_cwnd.py

# Student 4 — assemble submission
cd ~/Group4_Submission && zip -r ../Group4_TNN4113_Submission.zip .
```

---

**You've got this. Good luck.**
