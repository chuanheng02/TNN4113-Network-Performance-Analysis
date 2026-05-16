# TNN4113 Computer Networks — Group 4

## Network Performance Analysis Using the NS-3 Simulator

| | |
|---|---|
| **Course** | TNN4113 Computer Networks |
| **Project Title** | Network Performance Analysis Using NS-3 Simulator |
| **Client** | TechCampus Solutions |
| **Submission Date** | 21 May 2026 |
| **Group** | Group 4 |

---

## Group Members

| Student | Name | Matric ID | Task |
|---|---|---|---|
| S1 | Brendan Chan Kah Le | 83403 | Task 1 — Point-to-Point Link Analysis |
| S2 | Xavier Liong Zhi Hao | 86079 | Task 2 — CSMA LAN Contention Study |
| S3 | Ng Clarence Chuan Hann | 84832 | Task 3 — TCP Congestion Control (Dumbbell) |
| S4 | Chong Ming Zin | 83489 | Task 4 — Synthesis & Report |

---

## Project Overview

TechCampus Solutions reported three persistent network problems: delayed responses on direct links, dropped packets on the office LAN, and unfair bandwidth allocation between users. This project uses the **NS-3 discrete-event network simulator** to investigate each problem in isolation across three controlled experiments.

| Task | Layer Tested | Topology | Key Metric |
|---|---|---|---|
| Task 1 | Physical / Link | Point-to-Point | TCP throughput vs delay & error rate |
| Task 2 | MAC | 10-node CSMA LAN | PDR vs number of concurrent senders |
| Task 3 | Transport | Dumbbell | cWnd evolution: NewReno vs Cubic |

---

## Simulation Environment

> Taken from project report Section 4.1 — all members must use these exact versions.

| Item | Version |
|---|---|
| Operating System | Ubuntu 22.04 LTS (WSL2 or native) |
| NS-3 Release | **ns-3.40** |
| Compiler | g++ 11.4, C++17 |
| Build System | CMake via `./ns3` wrapper |
| NetAnim | 3.108 |
| Plotting | gnuplot 5.4 / Python 3.10 + Matplotlib 3.7 |
| RNG Seed | `RngSeedManager::SetSeed(1)` |
| Runs per point | 5 seeds (RngRun 1–5) |

---

## Repository Structure

```
Group4_TNN4113/
│
├── README.md                          ← this file
│
├── src/
│   ├── task1_p2p.cc                   ← Student 1 — P2P simulation
│   ├── task2_csma.cc                  ← Student 2 — CSMA LAN simulation
│   └── task3_dumbbell.cc              ← Student 3 — Dumbbell TCP simulation
│
├── scripts/
│   ├── task1_sweep.sh                 ← Student 1 — runs all 20 combinations
│   ├── plot_task1.py                  ← Student 1 — generates Figure 5.2
│   ├── plot_task2.py                  ← Student 2 — generates Figure 6.2
│   ├── plot_cwnd.py                   ← Student 3 — generates Figure 7.2
│   └── plot_cwnd.gp                   ← Student 3 — gnuplot alternative
│
├── results/
│   ├── task1_results.csv              ← 20-row sweep output (Student 1)
│   ├── task2_csma.xml                 ← NetAnim trace, 5-sender run (Student 2)
│   ├── cwnd-newreno.dat               ← NewReno cWnd time-series (Student 3)
│   ├── cwnd-cubic.dat                 ← Cubic cWnd time-series (Student 3)
│   ├── figure_5_2.png                 ← Throughput vs Delay & Error Rate
│   ├── figure_6_2.png                 ← CSMA throughput & PDR bar chart
│   └── cwnd_comparison.png            ← NewReno vs Cubic cWnd plot
│
├── screenshots/
│   ├── 5.1_task1_run.png              ← Terminal: Task 1 successful run
│   ├── 6.1_netanim.png                ← NetAnim: 10-node CSMA animation
│   ├── 6.2_task2_terminal.png         ← Terminal: Task 2 FlowMonitor output
│   └── 7.1_task3_run.png              ← Terminal: Task 3 run + .dat files
│
└── report/
    ├── TNN4113_Project_Report.docx    ← Final report (Student 4 owns)
    └── TNN4113_Project_Report.pdf     ← PDF export for submission
```

---

## How to Run Each Simulation

### Setup (everyone, once)

```bash
# 1. Install dependencies
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

pip3 install --user matplotlib numpy

# 2. Download and build NS-3.40
cd ~
wget https://www.nsnam.org/releases/ns-allinone-3.40.tar.bz2
tar xjf ns-allinone-3.40.tar.bz2
cd ns-allinone-3.40
./build.py --enable-examples --enable-tests

# 3. Verify
cd ns-3.40
./ns3 --version       # must print: ns-3.40
./ns3 run hello-simulator  # must print: Hello Simulator
```

---

### Task 1 — Student 1

```bash
cd ~/ns-allinone-3.40/ns-3.40

# Copy source
cp /path/to/src/task1_p2p.cc scratch/

# Single test run
./ns3 build
./ns3 run "scratch/task1_p2p --delay=10ms --errRate=0.00"

# Full 20-combination sweep
cp /path/to/scripts/task1_sweep.sh .
chmod +x task1_sweep.sh
./task1_sweep.sh
# Output: task1_results.csv

# Plot Figure 5.2
cp /path/to/scripts/plot_task1.py .
python3 plot_task1.py
# Output: figure_5_2.png
```

---

### Task 2 — Student 2

```bash
cd ~/ns-allinone-3.40/ns-3.40

# Copy source
cp /path/to/src/task2_csma.cc scratch/

# Build
./ns3 build

# Run all three scenarios
./ns3 run "scratch/task2_csma --nSenders=1" | tee task2_n1.log
./ns3 run "scratch/task2_csma --nSenders=3" | tee task2_n3.log
./ns3 run "scratch/task2_csma --nSenders=5" | tee task2_n5.log

# Save the 5-sender NetAnim XML
cp task2_csma.xml task2_csma_5senders.xml

# Open NetAnim for screenshot
~/ns-allinone-3.40/netanim-3.108/NetAnim
# Open task2_csma.xml → Play → screenshot while arrows visible

# Plot Figure 6.2
cp /path/to/scripts/plot_task2.py .
# Edit real numbers in the script first, then:
python3 plot_task2.py
# Output: figure_6_2.png
```

---

### Task 3 — Student 3

```bash
cd ~/ns-allinone-3.40/ns-3.40

# Copy source
cp /path/to/src/task3_dumbbell.cc scratch/

# Build and run
./ns3 build
./ns3 run scratch/task3_dumbbell
# Output: cwnd-newreno.dat, cwnd-cubic.dat

# Verify output files
ls -l cwnd-newreno.dat cwnd-cubic.dat

# Plot Figure 7.2
cp /path/to/scripts/plot_cwnd.py .
python3 plot_cwnd.py
# Output: cwnd_comparison.png

# Extract Table 7.1 drop/recovery data
python3 << 'EOF'
import numpy as np
for label, fn in [('NewReno','cwnd-newreno.dat'),('Cubic','cwnd-cubic.dat')]:
    d = np.loadtxt(fn)
    t, cwnd = d[:,0], d[:,1]
    drops = []
    for i in range(1, len(cwnd)):
        if cwnd[i] < cwnd[i-1] * 0.95:
            wmax, wmin, td = cwnd[i-1], cwnd[i], t[i]
            rt = next((t[j]-td for j in range(i+1,len(cwnd)) if cwnd[j]>=wmax), None)
            drops.append((td, wmax, wmin, rt))
            if len(drops) == 3: break
    print(f"\n{label}")
    for k,(td,wmx,wmn,rt) in enumerate(drops,1):
        print(f"  Drop {k}: W_max={int(wmx)}  W_min={int(wmn)}  Recovery={f'{rt:.3f}s' if rt else 'N/A'}")
EOF
```

---

## Submission Package

```bash
mkdir -p ~/Group4_Submission/{source_code,trace_outputs,plots,screenshots}

cp src/*.cc                              ~/Group4_Submission/source_code/
cp results/task1_results.csv            ~/Group4_Submission/trace_outputs/
cp results/task2_csma.xml               ~/Group4_Submission/trace_outputs/
cp results/cwnd-newreno.dat             ~/Group4_Submission/trace_outputs/
cp results/cwnd-cubic.dat               ~/Group4_Submission/trace_outputs/
cp results/figure_5_2.png               ~/Group4_Submission/plots/
cp results/figure_6_2.png               ~/Group4_Submission/plots/
cp results/cwnd_comparison.png          ~/Group4_Submission/plots/
cp screenshots/*.png                    ~/Group4_Submission/screenshots/
cp report/TNN4113_Project_Report.docx   ~/Group4_Submission/
cp report/TNN4113_Project_Report.pdf    ~/Group4_Submission/

cd ~
zip -r Group4_TNN4113_Submission.zip Group4_Submission/
```

Upload `Group4_TNN4113_Submission.zip` to eLeap before **21 May 2026**.

---

## References

1. G. F. Riley and T. R. Henderson, "The ns-3 Network Simulator," Springer, 2010.
2. ns-3 Project, "ns-3 Manual, Release ns-3.40," 2024. https://www.nsnam.org/docs/release/3.40/manual/html/
3. M. Mathis et al., "The macroscopic behavior of the TCP congestion avoidance algorithm," ACM SIGCOMM CCR, 1997.
4. S. Ha, I. Rhee, L. Xu, "CUBIC: A new TCP-friendly high-speed TCP variant," ACM SIGOPS, 2008.
5. S. Floyd and T. Henderson, "The NewReno Modification to TCP's Fast Recovery Algorithm," RFC 6582, 2012.
6. IEEE Std 802.3-2018, "IEEE Standard for Ethernet," 2018.
7. J. F. Kurose and K. W. Ross, Computer Networking: A Top-Down Approach, 8th ed., Pearson, 2021.
