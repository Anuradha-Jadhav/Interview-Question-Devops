### Linux Booting Process

The Linux booting process involves multiple stages, beginning from powering on the system to reaching a functional operating system. The process follows these steps:

---

### **1. BIOS/UEFI Initialization**
- When the system is powered on, the **BIOS (Basic Input/Output System)** or **UEFI (Unified Extensible Firmware Interface)** is executed.
- It performs a **Power-On Self Test (POST)** to check hardware components like RAM, CPU, and storage devices.
- BIOS locates the bootable device and loads the **bootloader** into memory.

---

### **2. Bootloader (GRUB)**
- The bootloader is responsible for loading the kernel into memory.
- The most commonly used bootloader in Linux is **GRUB (Grand Unified Bootloader)**.
- It provides a menu to select different kernels or operating systems (in dual-boot scenarios).
- The bootloader loads the **Linux kernel** into memory and passes control to it.

---

### **3. Linux Kernel Initialization**
- The kernel is the core of the operating system.
- It initializes:
  - CPU, memory, and device drivers.
  - Mounts the root filesystem (`/`).
  - Starts the **init system** (e.g., `systemd`).

---

### **4. Init/Systemd Process**
- The **init system** (traditionally `init`, now commonly `systemd`) is the first user-space process with **PID 1**.
- It executes startup scripts, manages services, and sets up the system environment.

**In modern Linux distributions:**
- **Systemd** initializes the system using **unit files**.
- It parallelizes the startup process for faster booting.

---

### **5. Runlevel/Target Initialization**
- The system reaches a specific **runlevel** (in `SysVinit`) or **target** (in `systemd`).
- Common runlevels in **SysVinit**:
  - `0` - Halt
  - `1` - Single-user mode
  - `3` - Multi-user mode (without GUI)
  - `5` - Multi-user mode (with GUI)
  - `6` - Reboot

- Common **systemd targets**:
  - `multi-user.target` (Equivalent to Runlevel 3)
  - `graphical.target` (Equivalent to Runlevel 5)

---

### **6. User Login and Shell Initialization**
- The system presents a login prompt (`tty` for CLI or a graphical login manager for GUI).
- Users can authenticate and access the system.

---

### **Booting Process Summary**
1. **BIOS/UEFI** → Initializes hardware and finds the bootloader.
2. **Bootloader (GRUB)** → Loads the Linux kernel.
3. **Kernel Initialization** → Detects hardware, mounts root filesystem.
4. **Init/Systemd** → Manages system services and processes.
5. **Runlevel/Target Initialization** → Sets up system mode.
6. **User Login** → System is fully operational.

Would you like a **Markdown file (`booting_process.md`)** summarizing this?
