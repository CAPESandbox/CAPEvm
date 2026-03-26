# CAPEvm: CAPE Virtual Machine

### 🧪 Overview
**CAPEvm** is a pre-configured CAPE virtual machine optimized to run natively on a **bare KVM hypervisor** alongside target VMs.

---

### 🏗️ Architecture & Intent
The recommended deployment model follows a **Flat-Hypervisor Topology**:
1.  **Host:** A bare-metal Linux server (Ubuntu 24.04 recommended).
2.  **Hypervisor:** KVM/QEMU installed via the official CAPE utility script kvm-qemu.sh
3.  **Guests:** This CAPE vm running in parallel with your Windows analysis VMs, remotely managing the host hypervisor via 'kvmremote' machinery using libvirt.

---

### 🚀 Installation & Setup

#### 1. Prepare the Hypervisor
We recommend using the official CAPE hypervisor installation script kvm-qemu.sh to set up the KVM hypervisor base.

    # On your Host Machine
    git clone https://github.com/kevoreilly/CAPEv2.git
    cd CAPEv2/installer
    sudo ./kvm-qemu.sh

#### 2. Download and Recombine the Image
Due to GitHub file size limits, the .qcow2 image is distributed as a multi-part archive.

1.  Download all parts (capevm.7z.001, capevm.7z.002, etc.) from the Releases tab.
2.  Recombine and extract using the first volume:

    # Ensure p7zip-full is installed
    7z x capevm.7z.001

    Note: 7zip will automatically detect and join the subsequent parts (.002, .003, etc.) as long as they are in the same directory.

#### 3. Define the VM in Libvirt
Move the extracted capevm.qcow2 to your libvirt storage pool (standard path: /var/lib/libvirt/images) and define it using virt-manager or virsh. 

> **Critical Note:** Ensure the network interface is attached to the same internal bridge as your Windows analysis VMs to allow for cross-guest communication and routing during multi-stage detonations.

---

### ⚙️ Configuration
Update kvmremote.conf to include the target VMs on your hypervisor in the form:

    [win10-1]
    label = win10-1
    platform = windows
    ip = 192.168.1.11
    tags = x64,win10
    snapshot = Ready
    arch = x64

---


#### Hyper-V Support
For Windows-based environments, this VM can be utilized by:
1. Converting the .qcow2 to .vhdx via qemu-img.
2. Switch the CAPE configuration to use the **Hyper-V Machinery** module (hyperv).
3. Configure your hyperv.conf machinery module configuration

#### VMware
The image can be converted to .vmdk for use in ESXi or Workstation environments if required.

---

**© 2026 CAPE Sandbox Contributors**
