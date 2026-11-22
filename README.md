# 🐧 Linux Ops Arena — Kubernetes-Ready Linux & DevOps Quiz Game

**Linux Ops Arena** is an interactive, single-file Linux & DevOps speed-run quiz game and study platform built specifically to be containerized and deployed seamlessly on Kubernetes (or any static web server).

Master **420 curated Linux questions** across **7 core system administration domains** and **3 progressive difficulty tiers** while defending your cluster health against production incidents!

---

## ✨ Key Features

* **420 Total Questions (20 per Topic × 3 Levels):** Every single topic includes 20 Basic, 20 Intermediate, and 20 Advanced questions (`60 questions per topic × 7 topics = 420 questions`).
* **Dual Learning & Gameplay Modes:**
  * **⚡ Arena Run Mode:** Timed speed-run mode with a live countdown timer (24s Basic, 18s Intermediate, 14s Advanced), streak multipliers (up to `5x`), cluster health meter, and animated Pod-Runner rollout tracker.
  * **📚 Question Bank Mode:** Browse, search, and review all 20 questions, correct answers, and detailed technical explanations for any topic and difficulty level.
* **Built-In Kubernetes Manifests Drawer (`☸️ K8s Manifests`):** View and one-click copy ready-to-use `deployment.yaml`, `service.yaml`, and `configmap-nginx.yaml` directly from the UI.
* **Zero External Backend Required:** Single self-contained HTML file (`index.html`) with Web Audio API synthesized sound effects and `localStorage` persistence for high scores, streaks, accuracy, and mastered question tracking.

---

## 🗂️ Covered Linux Topics (7 Domains)

| Code | Topic | Key Concepts Covered |
| :--- | :--- | :--- |
| **USR** | Users & Permissions | `useradd`, `usermod`, `chown`, `chmod`, `visudo`, SUID/SGID, Sticky Bit, POSIX ACLs (`setfacl`), `chattr`, PAM, `umask` |
| **LVM** | LVM & Storage | `pvcreate`, `vgcreate`, `lvcreate`, `lvextend -r`, `xfs_growfs`, LVM snapshots & merging, `pvmove`, `/etc/fstab` UUIDs, thin pools |
| **FSM** | File System & Nav | FHS hierarchy, `df -h`, `df -i` (inodes), `du -sh`, `find`, hard vs. symbolic links, `rsync`, `tune2fs`, `fuser`, `/lost+found` |
| **SYS** | Process & Systemd | `systemctl`, `journalctl`, drop-in overrides, `daemon-reload`, masking units, `SIGTERM` vs `SIGKILL`, zombie processes, cgroups/OOM, `renice` |
| **NET** | Networking & Firewall | `ss -tulpn`, `ip addr`, `ip route`, `dig`, `ufw`, `firewalld`, `tcpdump`, `iptables` chains, `net.ipv4.ip_forward`, network namespaces (`ip netns`) |
| **SHL** | Shell & Text Ops | Bash shebangs, `grep`, `awk`, `sed`, pipes & redirection (`2>&1`), `set -euo pipefail`, parameter expansion, `xargs -0`, `trap` |
| **KRN** | Package & Kernel/Boot | `uname`, `apt`, `dnf`, `sysctl`, `lsmod`, `modprobe`, `initramfs`, GRUB2 config, `dmesg -T`, `vm.swappiness`, `ldconfig` |

---

## 🕹️ Controls & Keyboard Shortcuts

* **Select Answer:** Click an option or press `1`–`4` / `A`–`D` on your keyboard.
* **Next Question:** Click **Next Challenge →** or press `Enter`.
* **Mode & Settings:** Switch between **Arena Run**, **Question Bank**, **K8s Manifests**, toggle **Sound FX** (`🔊`/`🔇`), or **Reset** saved progress from the top navigation bar.

---

## ☸️ Deploy on Kubernetes

### Option 1: Using Repository Manifests (ConfigMap + Nginx)

Deploy directly using the manifests in this repository (`configmap-nginx.yaml`, `deployment.yaml`, and `service.yaml`):

```bash
# 1. Create namespace
kubectl create namespace devops-arena

# 2. Load index.html into a ConfigMap (or apply configmap-nginx.yaml)
kubectl create configmap linux-ops-arena-html \
  --from-file=index.html=index.html \
  -n devops-arena

# 3. Apply Deployment and Service
kubectl apply -f deployment.yaml -n devops-arena
kubectl apply -f service.yaml -n devops-arena

Access the game at http://<NODE_IP>:30080 or port-forward locally:
kubectl port-forward svc/linux-ops-arena-svc 8080:80 -n devops-arena
