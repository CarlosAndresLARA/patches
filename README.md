# patches
**Some patches for the linux kernel**

*These patches can be used to produce a DVFS-enabled linux kernel for gem5.*

**1. Get the sources:**

```
mkdir kernel; cd kernel
git clone https://github.com/CarlosAndresLARA/patches.git
git clone https://gem5.googlesource.com/arm/linux linux-gem5.git
git clone https://github.com/linaro-swg/linux.git
git -C linux checkout optee-3.12.0
```

**2. Patch the cpufreq driver:**

```
rm -r linux/drivers/cpufreq; cp -r linux-gem5/drivers/cpufreq linux/drivers/;
rm linux/include/linux/cpufreq.h; cp linux-gem5/include/linux/cpufreq.h linux/include/linux/;
rm linux/include/linux/sched/cpufreq.h; cp linux-gem5/include/linux/sched/cpufreq.h linux/include/linux/sched/;
cp linux-gem5/include/linux/gem5_energy_ctrl.h linux/include/linux/
patch linux/include/linux/cpufreq.h < patches/cpufreq.h.patch;
patch linux/include/linux/sched/cpufreq.h < patches/sched_cpufreq.h.patch;
patch linux/drivers/cpufreq/cpufreq.c < patches/cpufreq.c.patch;
patch linux/drivers/cpufreq/cpufreq_governor.c < patches/cpufreq_governor.c.patch;
patch linux/drivers/cpufreq/cpufreq-dt.c < patches/cpufreq-dt.c.patch;
patch linux/drivers/cpufreq/qoriq-cpufreq.c < patches/qoriq-cpufreq.c.patch;
patch linux/drivers/cpufreq/arm_big_little.c < patches/arm_big_little.c.patch
```

**3. Patch the clk driver:**

```
cp -r linux-gem5/drivers/clk/gem5/ linux/drivers/clk/
patch linux/drivers/clk/Kconfig < patches/Kconfig.patch;
patch linux/drivers/clk/Makefile < patches/Makefile.patch
```

**4. Patch other kernel files:**

```
cp linux-gem5/kernel/m5struct.c linux/kernel/;
patch linux/kernel/Makefile < patches/Kernel_Makefile.patch
cp linux-gem5/arch/arm64/configs/gem5_defconfig linux/arch/arm64/configs/
```

**That's it!**
