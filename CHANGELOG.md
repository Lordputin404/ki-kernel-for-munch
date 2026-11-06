# Ki-kernel for POCO F4 / Redmi K40S (munch)

**Build Date:** 2026-11-06  
**Kernel Version:** Linux 4.19.325  
**Variant:** MIUI/HyperOS, AOSP, And ColorOS/OxygenOS (Universal & Single Variants)  
**Toolchain:** ZyCromerZ Clang 16.0.6 (LLVM 16.0.6 + GNU Binutils 2.47)  

---

## Ki-kernel v1.6.2 (ColorOS 17 Support & Installer Bulletproofing) :
- **ColorOS 17 Display Subsystem:** Implemented `/sys/kernel/oplus_display/` (with `/sys/kernel/oppo_display` symlink) providing `adfr`, `dimlayer_bl_en`, `dimlayer_hbm`, `seed`, `hbm`, `oplus_notify_fppress`, `power_status`, `panel_serial_number`, `dynamic_osc_clock`, `aod_light_mode_set`, `panel_pwr`, and `dsi_cmd` to support ColorOS 17 Luminous Rendering engine without SurfaceFlinger hangs.
- **ColorOS 17 Background Freezer:** Added `/proc/oplus_hans/` (`hans_ctrl`, `hans_type`, `hans_stat`, `pkg_list`) interface to eliminate Hans v5 system server ANRs and lockups.
- **ColorOS 17 Horae Thermal Engine:** Added `/proc/shell-temp` (dynamically reporting battery shell thermals) and `/proc/horae_mode` to prevent thermal daemon throttling and false overheat warnings.
- **ColorOS SuperVOOC UI Charging Nodes:** Registered `voocchg_ing`, `svoocchg_ing`, `svooc_100w_icon`, and `adapter_fw_update` in `/sys/class/oplus_chg/` alongside dynamic `fast_chg_type` for authentic charging animations.
- **Device Info & Bootprof:** Added `/proc/opbootfrom` and `/proc/devinfo/` (`aboard`, `ddr_info`, `ddr_size`).
- **SchedAssist v3 & Frame Boost:** Expanded `/proc/oplus_scheduler/sched_assist/` (`opt_enable`, `frame_boost`, `sf_target_time`, `inherit_ux`, `heavy_task`, `sched_fbg_opt`, `fbg_slide_opt`, `fbg_camera_opt`, `fbg_anim_opt`, `sched_assist_v2`) and `/proc/oplus_frame_boost/` (`fbg_ctrl`, `fbg_fps`, `fbg_level`, `fbg_pid`, `fbg_render_pid`, `fbg_mode`, `fbg_scene`).
- **OPlus Theia & GameOpt:** Added `/proc/theia/theia_send_event` watchdog reporter and `/proc/game_opt/` (`render_thread`, `game_pid`, `cpu_load`, `target_fps`).
- **OPlus Charger & Gauge:** Registered `/sys/class/oplus_chg/` wireless and gauge sysfs device groups (`chip_ok`, `soh`) and added `/proc/oplus_charger/` battery metrics (`battery_temp`, `ui_soc`, `charge_technology`, `gauge_fw_ver`).
- **AnyKernel3 Partition Resolution:** Fixed `"Unable to determine boot partition. Aborting..."` error when flashing via root apps (Kernel Flasher / FKM) on Virtual A/B by adding multi-layer slot detection fallback (`/proc/mounts`, `/proc/self/mountinfo`, and direct block device scans) and broadening path resolution to include `/dev/block/mapper/` and wildcards.
- **AnyKernel3 Environment Hardening:** Fixed empty `$AKHOME` directory evaluation during third-party app installations and synchronized dual-case variables (`BLOCK`/`block`, `IS_SLOT_DEVICE`/`is_slot_device`).
- **ROM Auto-Detection:** Added `ro.oplus.image.my_manifest.version` to OPlus ROM fingerprint scanner.

## Ki-kernel v1.6.1 :
- Fixed Samsung E4 AMOLED black screen bug: permanently protected display power rails (`display_panel_vci`, `disp_vddio_vreg`, `oled_dvdd_ldo`) from being cut off at second 32 by `regulator_late_cleanup`, completely eliminating hard reboots
- Fixed AnyKernel3 Universal installer: initialized `$AKHOME` directory binding to resolve `! Kernel Image not found` abort during recovery flashing
- Added built-in fgetpriority UID priority hiding to defeat syscall-based app detection (Applist Detector/Momo)
- Accelerated display & frame sync: speed up sync_file ioctl by omitting debug names (improves SurfaceFlinger and gaming FPS)
- Optimized memory zeroing: align clear_page() to 16 bytes for Cortex-A77/A55 cacheline throughput
- Silenced smcinvoke false error logspam during suspend/resume
- Fixed ARM PMU and devfreq memlat random reboots and qspinlock lockups
- Restored stable v1.5 display KMS commit order and touch synchronization
- Restored stable v1.5 kswapd LITTLE cluster affinity (CPUs 0-3) and watermark scale factor (improves gaming stability and prevents aggressive LMK oom_reaper)

## Ki-kernel v1.6 :
- Fix Selinux
- Updated ADIOS I/O scheduler v3.3.0
- Added AnyKernel GPU selector 
- Added YukiSU manager support and YukiSU feature
- Synced SuSFS v2.3.0 upstream changes and fixed UID filter UAF race conditions
- Added SuSFS multi-user and dual apps UID support with no-atime parsing
- Fixed Dynamic Fsync screen-off suspend hangs
- Fixed Binder page allocation failures and improved kswapd sleep reclaim
- Fixed cpufreq policy rwsem suspend deadlocks
- Fixed cpu-boost touch workqueue deadlocks and reduced policy lock contention
- Fixed Wakelock Blocker PCIe Wi-Fi drops and USB/ADB disconnects
- Fixed Ki-Profile screen-off workqueue loops and deep sleep wakeups

## Ki-kernel v1.5 :
- Updated BakaSU and ReSukiSU to UAPI v6
- Added full FinaSU Manager support
- Added 32-bit userspace KernelSU compatibility
- Added kernel-side per-app network isolation
- Synced SuSFS v2.3.0 inline hooks and bugfixes
- Added automated SuSFS early-boot spoofing
- Added SuSFS priority UID filter protection
- Updated NoMount kernel integration to v2.2.0
- Fixed Kernel Manager sysfs SELinux denials
- Ported hardware AOSP DC Dimming support
- Fixed display sysfs SELinux permission issues
- Improved SDE KCAL hardware color calibration
- Fixed Force Fast Charge on wall adapters
- Restored full 67W PPS turbo charging
- Fixed random thermal shutdowns while charging
- Fixed duplicate charger sysfs registration warnings
- Tuned Battery, Balanced, and Performance profiles
- Tuned CPU scheduling and gaming frame pacing
- Fixed screen-off wakeup freezes and AOD issues
- Hardened Ki-Profile sysfs and worker stability
- Fixed Adreno GPU freezes and Sleep of Death
- Fixed MGLRU lock contention and app freezes
- Set BFQ and Maple as default I/O schedulers
- Improved BBRv3 network congestion control
- Fixed USB tethering and DHCP connection issues
- Improved Droidspaces and Linux container support
- Added OPlus Frame Boost and SchedAssist support
- Fixed OPlus port netlink and driver issues
- Improved AnyKernel3 universal Multi-ROM detection

## Ki-kernel v1.4.1:
• Backported modern eBPF features for Android 16 and 17
• Cherry-picked selected commits from re-noroi/FD and e404
• Fixed BPF loader bootloops on newer Android ROMs
• Added dynamic kernel version reporting for BPF loaders
• Added OPlus subsystem support for ColorOS, OxygenOS and RealmeUI ports
• Added OPlus NANDswap and charging subsystem compatibility
• Improved AnyKernel3 ROM detection for AOSP, MIUI, HyperOS and OPlus ports
• Fixed Sleep of Death and screen-off hangs
• Fixed CPU governor switch kernel panics and use-after-free crashes
• Restored 300MHz CPU idle frequency
• Tuned schedutil, WALT, CPU migration and hispeed thresholds
• Added per-cluster CPU frequency controls in Ki-Profile
• Improved Battery Saver and Balanced profile tuning
• Improved screen-off battery life and dynamic refresh rate handling
• Added thermal auto-guard with temperature-based protection
• Improved CPU and GPU stability under heavy loads
• Improved bypass charging safety and charger disconnect handling
• Fixed fast charging cutoff, thermal limits and charging state handling
• Set Force Fast Charge and Screen-On Fast Charge to OFF by default
• Added non-root controls for Ki-Profile and charging features
• Expanded KernelSU manager compatibility
• Fixed KernelSU root detection and restored SuSFS inline hooks
• Added Boeffla Wakelock Blocker with a configurable toggle
• Fixed background music playback stopping when the screen is off
• Improved memory management with MGLRU and LMKD tuning
• Fixed excessive kswapd activity under heavy ZRAM usage
• Improved app retention and reduced unnecessary background app kills
• Added Adreno 650 GPU idle frequency reporting
• Retained Adreno 650 GPU undervolt and overclock profiles
• Fixed DC Dimming support on AOSP and compatible AMOLED panels
• Fixed Night Light and LiveDisplay on supported custom ROMs
• Added KCAL hardware color calibration support
• Added Dynamic Fsync control
• Added ADIOS multi-queue I/O scheduler
• Added NoMount kernel integration
• Added native Linux container support for Droidspaces, LXC and Docker
• Added BINFMT_MISC binary emulation support
• Backported Futex2 multi-wait support
• Added EROFS compression and security xattr support
• Improved block I/O and asynchronous bio submission
• Improved CPU locking and memory mapping performance
• Tuned real-time timer handling for SurfaceFlinger, audio and GPU workers
• Fixed touchscreen sleep errors and display-related issues
• Fixed IR blaster transmission issues
• Improved boot image ramdisk decompression and AOSP boot compatibility
• Added a lockless per-CPU Blake2s random number generator
• Updated Clang ThinLTO and fixed vendor module loading
• Added multi-stage thermal, charging and screen-off profile handling
• Improved container networking and systemd cgroup compatibility

---

## Ki-kernel v1.4

Changelog:
• Backported modern eBPF support for Android 16 and 17
• Fixed BPF loader boot issues on newer Android ROMs
• Added dynamic kernel version reporting for BPF loaders
• Added Adreno 650 GPU idle frequency reporting
• Retained custom GPU undervolt and overclock profiles
• Updated BakaSU and ReSukiSU integration
• Fixed KernelSU Manager root detection
• Synced SuSFS v2.3.0 inline hooks
• Added non-root Ki-Profile controls
• Added non-root bypass charging controls
• Added Force Fast Charge controls
• Added universal DC Dimming support
• Added KCAL hardware color calibration
• Added Dynamic Fsync controls
• Tuned CPU scheduling and gaming frame pacing
• Improved RAM reclaim and multitasking behavior
• Improved Deep Sleep and background power consumption
• Fixed screen-off charging freezes and Sleep of Death
• Fixed AOSP boot freezes and kernel panic issues
• Fixed lockscreen and display refresh-rate issues
• Improved battery protection and bypass charging handling
• Added native Linux container support
• Added NoMount kernel integration
• Added ADIOS I/O scheduler
• Added CAKE queue management support
• Added PS5 DualSense controller support
• Improved touchscreen and haptic driver handling
• Fixed background music playback during screen-off
• Improved AnyKernel3 flashing and DTB handling
• Unified support for Android 13–17 ROM builds

---

## v1.4
• Switched default ZRAM compression to LZ4
• Fixed hardware DC Dimming
• Tuned Ki-Profile for higher gaming performance
• Improved CPU migration and schedutil response
• Tuned RAM, VM and kswapd behavior
• Improved Force Fast Charge and Screen-On Fast Charge
• Set ZRAM and system swappiness to 90
• Backported Binder TF_CLEAR_BUF support
• Added USB UAS support
• Added NTFS filesystem support
• Fixed CPU idle frequency and wake-from-idle issues
• Fixed MIUI and AnyKernel3 flashing issues
• Backported epoll_pwait2 and improved TCP Westwood
• Optimized CFS load balancing
• Optimized Focaltech touch and aw86927 haptic handling
• Removed Adreno POPP throttling and Wi-Fi debug wakelocks
• Added Boeffla Wakelock Blocker
• Improved Deep Sleep and suspend behavior
• Added Virtual A/B snapshot support
• Tuned F2FS background GC
• Improved GPU and touchscreen thread scheduling
• Enabled Clang ThinLTO
---

## v1.3
• Added in-kernel ki_profile switcher (Battery / Balanced / Performance)
• Added Dynamic Fsync
• Migrated to ReSukiSU v4.2.0
• Backported NTSYNC driver
• Upstream SUSFS v2.3.0
• Improved CPU idle frequency handling
• Added I/O wait boost limits
• Fixed Wi-Fi hotspot and tethering
• Improved bypass charging handling
• Migrated to ZyCromerZ Clang 16.0.6

---
  <a href="https://t.me/+i7NfuXZA6mZjOWJl">
    <img src="https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram">
  </a>

