📦 Containers vs 🖥️ Virtual Machines — Full Summary
🧱 WHAT IS CONTAINERIZATION?
Containerization is a lightweight virtualization technique where applications run in isolated user spaces, but share the host’s OS kernel.

Containers include only what's needed to run: the app code, dependencies, and config.

Tools: Docker, Podman, containerd, etc.

🖥️ WHAT IS A VIRTUAL MACHINE?
A Virtual Machine (VM) is a full-blown operating system running on virtualized hardware, completely isolated from the host via a hypervisor.

Each VM includes its own kernel, system libraries, and applications.

🔍 KEY DIFFERENCES: Container vs VM
Feature	Containerization	Virtual Machine
OS Kernel	Shared with host	Separate kernel per VM
Startup Speed	Very fast (seconds)	Slow (minutes)
Performance	Lightweight, low overhead	Heavyweight, more resource usage
Image Size	Small (MBs)	Large (GBs)
Isolation	Process-level	Hardware-level
Portability	High	Moderate
🖥️ Container Behavior on Ubuntu vs Windows
✅ On Linux/Ubuntu Host:
Docker runs natively using the Linux kernel — no VM required.

Very efficient and performant.

🪟 On Windows Host:
Docker Desktop creates a VM using WSL2 or Hyper-V to run Linux containers.

To run Windows containers, the host Windows kernel is used directly.

Even for Linux containers, a VM layer is essential on Windows.

🗃️ Base Image Examples
Linux-based container images:
ubuntu, alpine, debian, centos, node, nginx, etc.

Windows-based container images:
mcr.microsoft.com/windows/servercore

mcr.microsoft.com/windows/nanoserver

⚠️ You cannot run Linux container images on a native Windows kernel, and vice versa.

🤔 Is Linux Installed in a Container?
No full Linux OS is installed.

Only userland components (e.g., binaries, libraries) from the base image are used.

The container continues to rely on the host OS kernel.

🧨 CVE-2019-5736 — runc Container Escape Vulnerability
🛠️ What is runc?
runc is the low-level runtime used by Docker and other container platforms.

It’s a CLI tool that spawns and runs containers on the host machine.

🚨 Vulnerability Summary:
A malicious container could exploit runc and gain root access on the host.

Triggered when an admin uses docker exec.

🔥 How It Worked:
Malicious container starts.

Admin runs docker exec <container>.

Container overwrites /proc/self/exe, which points to runc.

Next container execution runs attacker's code on host as root.

🧯 Mitigation:
Update to a patched version of runc.

Avoid using docker exec on untrusted containers.

Use read-only containers, seccomp, AppArmor, and non-root users.

🛡️ Other Notable Container Vulnerabilities
1. Dirty Pipe (CVE-2022-0847)
Linux kernel bug allowing file overwrite.

Can be used by containers to escalate privileges.

2. Docker Socket Exposure
Mounting /var/run/docker.sock gives full control of Docker.

Allows container to start other containers or control host Docker daemon.

3. Running Containers as Root
A common misconfiguration.

Increases risk of privilege escalation or container breakout.

✅ Best Practices for Container Security
Use non-root users (USER directive in Dockerfile).

Enable security profiles: seccomp, AppArmor, SELinux.

Don’t use --privileged unless absolutely necessary.

Avoid mounting the Docker socket.

Regularly update container runtimes and host kernel.

⚙️ WHO MANAGES WHAT? — Containers vs VMs
Technology	Used For	Managed By
Containers	Lightweight app-level isolation	Kubernetes, Docker Swarm, Nomad
VMs	Full OS virtualization	Hypervisors, Cloud Platforms
☸️ Container Orchestration — Kubernetes
Kubernetes is a powerful container orchestrator.

Automates:

Container deployment

Scaling

Networking and service discovery

Rolling updates and self-healing

Alternatives:
Docker Swarm

HashiCorp Nomad

🧰 VM Orchestration / Management
1. Hypervisors (Low-level):
Type 1 (bare-metal): VMware ESXi, Hyper-V, Xen

Type 2 (hosted): VirtualBox, VMware Workstation

2. Management Platforms:
vSphere – Enterprise VM infrastructure (VMware)

Proxmox VE – Open-source platform for VMs and containers

OpenStack – Cloud framework to manage VMs, networks, storage

Cloud Providers:

AWS EC2 + Auto Scaling Groups

Azure VM Scale Sets

Google Compute Engine (GCE)

✅ Summary Table
Layer	Container World	VM World
Runtime	Docker, containerd, CRI-O	Hypervisors (KVM, Xen, Hyper-V)
Orchestrator	Kubernetes, Swarm, Nomad	OpenStack, vSphere, Cloud Platforms