
## 1. Virtualization: Hypervisors Compared
A hypervisor is the "manager" that lets you run multiple Virtual Machines (VMs) on one physical computer. [4, 5] 

| Feature [4, 6, 7, 8, 9, 10, 11, 12, 13] | Type 1 (Bare-Metal) | Type 2 (Hosted) |
|---|---|---|
| Example | VMware ESXi | Oracle VM VirtualBox |
| Where it sits | Directly on the hardware | On top of an OS (Windows/Mac) |
| Performance | Best: Minimal overhead | Moderate: Lower efficiency |
| Security | Higher: Fewer layers to attack | Lower: Dependent on Host OS |
| Main Use | Data Centers & Enterprise | Students, Testing, & Devs |

------------------------------
## 2. Setting Up Your Environment

* For ESXi (Type 1):
* Requires a dedicated machine; it will wipe all data on the drive during installation.
   * Managed remotely via a web browser using the server's IP address. [14] 
* For VirtualBox (Type 2):
* Installed like a normal application on your current laptop.
   * Allows you to run a second OS (like Linux) in a window while you still use your main desktop. [15] 
* Pro-Tip: For both, you must enter your computer’s BIOS/UEFI and enable Virtualization Technology (VT-x or AMD-V) before they will function.

[4] [https://aws.amazon.com](https://aws.amazon.com/compare/the-difference-between-type-1-and-type-2-hypervisors/)
[5] [https://www.youtube.com](https://www.youtube.com/watch?v=v5P-HDucGAI)
[6] [https://www.geeksforgeeks.org](https://www.geeksforgeeks.org/system-design/hypervisor/)
[7] [https://aws.amazon.com](https://aws.amazon.com/compare/the-difference-between-type-1-and-type-2-hypervisors/)
[8] [https://hostkey.com](https://hostkey.com/blog/94-bare-metal-hypervisor-vs-hosted/)
[9] [https://www.studysmarter.co.uk](https://www.studysmarter.co.uk/explanations/computer-science/computer-systems/hypervisors/)
[10] [https://www.parallels.com](https://www.parallels.com/blogs/ras/types-of-hypervisor-in-cloud-computing/)
[11] [https://www.studocu.com](https://www.studocu.com/in/document/pillai-college-of-engineering/cloud-computing/cloud-computing-experiment-03-studying-bare-metal-hypervisor/27186231)
[12] [https://www.youtube.com](https://www.youtube.com/watch?v=0cAcYq7YyWQ&t=731)
[13] [https://aws.amazon.com](https://aws.amazon.com/what-is/hypervisor/)
[14] [https://www.logicmonitor.com](https://www.logicmonitor.com/blog/vmware-vsphere-vs-esxi-vs-vcenter)
[15] [https://www.youtube.com](https://www.youtube.com/shorts/5NlG1c48ugc)
