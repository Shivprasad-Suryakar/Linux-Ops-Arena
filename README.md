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
## 📂 Repository Structure

* `index.html` — Self-contained Linux Ops Arena web app
* `configmap-nginx.yaml` — Kubernetes ConfigMap mounting `index.html`
* `deployment.yaml` — Kubernetes Deployment (2 replicas of `nginx:1.27-alpine`)
* `service.yaml` — Kubernetes NodePort Service (port `80` → nodePort `30080`)
* `README.md` — Project documentation

---
---

## 🌐 Ways to Open & Play in Browser

### 1. Fork & Enable GitHub Pages
1. Click **Fork** at the top-right of this repository to copy it to your GitHub account.
2. In your forked repository, go to **Settings → Pages**.
3. Under **Build and deployment → Branch**, choose `main` (or `master`) and `/ (root)`, then click **Save**.
4. Open your published GitHub Pages link (`https://<your-username>.github.io/<repo>/` or `https://<your-username>.github.io/<repo>/index.html`).

### 2. Run Locally (Double-Click in Browser)
1. Download or save `index.html` onto your computer.
2. Open File Explorer and **double-click** `index.html` to launch it directly in your browser.

### 3. Netlify Drop (Instant Drag & Drop Hosting)
1. Save `index.html` in a folder on your computer.
2. Open **Netlify Drop** in your browser and drag-and-drop the file or folder.
3. Click the instant live URL provided to open the app in your browser.

---

## ☸️ Deploy on Kubernetes

**1. Generate or update `configmap-nginx.yaml` from `index.html`:**

```bash
kubectl create configmap linux-ops-arena-html --from-file=index.html=index.html --dry-run=client -o yaml > configmap-nginx.yaml
```

**2. Apply the ConfigMap, Deployment, and NodePort Service:**

```bash
kubectl apply -f configmap-nginx.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl rollout restart deployment/linux-ops-arena
```

**3. Verify pods and NodePort service:**

```bash
kubectl get pods -l app=linux-ops-arena
kubectl get svc linux-ops-arena-svc
```

**4. Access the game in your browser via your node's IP on NodePort `30080`:**

```text
http://<NODE_IP>:30080
```

**5. Or forward the service port locally (`http://localhost:8080`):**

```bash
kubectl port-forward svc/linux-ops-arena-svc 8080:80
```
