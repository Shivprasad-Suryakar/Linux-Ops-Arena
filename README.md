🐧 Linux Ops Arena — Kubernetes-Ready Linux & DevOps Quiz GameLinux Ops Arena is an interactive, single-file Linux & DevOps speed-run quiz game and study platform built specifically to be containerized and deployed seamlessly on Kubernetes (or any static web server).Master 420 curated Linux questions across 7 core system administration domains and 3 progressive difficulty tiers while defending your cluster health against production incidents!✨ Key Features420 Total Questions (20 per Topic × 3 Levels):Every single topic includes 20 Basic, 20 Intermediate, and 20 Advanced questions (60 questions per topic × 7 topics = 420 questions).Dual Learning & Gameplay Modes:⚡ Arena Run Mode: Timed speed-run mode with a live countdown timer (24s Basic, 18s Intermediate, 14s Advanced), streak multipliers (up to 5x), cluster health meter, and animated Pod-Runner rollout tracker.📚 Question Bank Mode: Browse, search, and review all 20 questions, correct answers, and detailed technical explanations for any topic and difficulty level.Built-In Kubernetes Manifests Drawer (☸️ K8s Manifests):View and one-click copy ready-to-use deployment.yaml, service.yaml, and configmap-nginx.yaml directly from the UI.Zero External Backend Required:Single self-contained HTML file (linux-ops-arena.html) with Web Audio API synthesized sound effects and localStorage persistence for high scores, streaks, accuracy, and mastered question tracking.🗂️ Covered Linux Topics (7 Domains)CodeTopicKey Concepts CoveredUSRUsers & Permissionsuseradd, usermod, chown, chmod, visudo, SUID/SGID, Sticky Bit, POSIX ACLs (setfacl), chattr, PAM, umaskLVMLVM & Storagepvcreate, vgcreate, lvcreate, lvextend -r, xfs_growfs, LVM snapshots & merging, pvmove, /etc/fstab UUIDs, thin poolsFSMFile System & NavFHS hierarchy, df -h, df -i (inodes), du -sh, find, hard vs. symbolic links, rsync, tune2fs, fuser, /lost+foundSYSProcess & Systemdsystemctl, journalctl, drop-in overrides, daemon-reload, masking units, SIGTERM vs SIGKILL, zombie processes, cgroups/OOM, reniceNETNetworking & Firewallss -tulpn, ip addr, ip route, dig, ufw, firewalld, tcpdump, iptables chains, net.ipv4.ip_forward, network namespaces (ip netns)SHLShell & Text OpsBash shebangs, grep, awk, sed, pipes & redirection (2>&1), set -euo pipefail, parameter expansion, xargs -0, trapKRNPackage & Kernel/Bootuname, apt, dnf, sysctl, lsmod, modprobe, initramfs, GRUB2 config, dmesg -T, vm.swappiness, ldconfig🕹️ Controls & Keyboard ShortcutsSelect Answer: Click an option or press 1–4 / A–D on your keyboard.Next Question: Click Next Challenge → or press Enter.Mode & Settings: Switch between Arena Run, Question Bank, K8s Manifests, toggle Sound FX (🔊/🔇), or Reset saved progress from the top navigation bar.☸️ Deploy on Kubernetes (2 Options)Option 1: Zero-Build Deployment using ConfigMap + Nginx (Fastest)Mount linux-ops-arena.html directly into an official nginx:1.27-alpine Pod without building a custom Docker image:# 1. Create namespace
kubectl create namespace devops-arena

# 2. Load the HTML game into a ConfigMap
kubectl create configmap linux-ops-arena-html \
  --from-file=index.html=linux-ops-arena.html \
  -n devops-arena

# 3. Apply Deployment and Service
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: linux-ops-arena
  namespace: devops-arena
  labels:
    app: linux-ops-arena
spec:
  replicas: 2
  selector:
    matchLabels:
      app: linux-ops-arena
  template:
    metadata:
      labels:
        app: linux-ops-arena
    spec:
      containers:
        - name: nginx-quiz
          image: nginx:1.27-alpine
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "50m"
              memory: "64Mi"
            limits:
              cpu: "200m"
              memory: "128Mi"
          volumeMounts:
            - name: game-html
              mountPath: /usr/share/nginx/html/index.html
              subPath: index.html
      volumes:
        - name: game-html
          configMap:
            name: linux-ops-arena-html
---
apiVersion: v1
kind: Service
metadata:
  name: linux-ops-arena-svc
  namespace: devops-arena
spec:
  type: NodePort
  selector:
    app: linux-ops-arena
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30080
EOF
Access the game at http://<NODE_IP>:30080 or port-forward locally:kubectl port-forward svc/linux-ops-arena-svc 8080:80 -n devops-arena
Then open http://localhost:8080 in your browser.Option 2: Build a Custom Docker ImageIf you prefer packaging the HTML inside a container image:DockerfileFROM nginx:1.27-alpine
COPY linux-ops-arena.html /usr/share/nginx/html/index.html
EXPOSE 80
Build & Deploy:docker build -t linux-ops-arena:v1 .
kubectl create deployment linux-ops-arena --image=linux-ops-arena:v1
kubectl expose deployment linux-ops-arena --type=NodePort --port=80
💻 Run Locally Without KubernetesOpen linux-ops-arena.html directly in any modern browser, or start a local HTTP server:python3 -m http.server 8080
Visit http://localhost:8080/linux-ops-arena.html.
