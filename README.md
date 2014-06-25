**Building the kernel for Samsung Galaxy S 5 Exynos 5422**

I assume your home directory is /home/user, kernel source inside
/home/user/android/android_kernel_samsung_universal5422

- **PART I: Building the kernel**

* Create an environtment text file, save it as /home/user/android/build.env (change CROSS_COMPILE to point to your toolchain):
```
export CROSS_COMPILE='/home/user/android/system/prebuilts/gcc/linux-x86/arm/arm-eabi-4.7/bin/arm-eabi-'
export LDFLAGS=''
export CFLAGS=''
export SUBARCH=arm
export ARCH=arm
```

* Import environtment file above, create a build directory outside the kernel source tree (to keep the kernel source clean):
```
$ cd /home/user/android/android_kernel_samsung_universal5422
$ source /home/user/android/build.env
$ mkdir -p /home/user/android/build/universal5422
$ make O=/home/user/android/build/universal5422 mrproper
```

* Import the kernel config file, change the kernel configuration (if needed) and start the building process:
```
$ make O=/home/user/android/build/universal5422 exynos5422-k3g_00_defconfig
$ make O=/home/user/android/build/universal5422 menuconfig
$ make -j8 O=/home/user/android/build/universal5422
```

* Copy the resulting kernel and modules (if available) to a binary directory:
```
$ mkdir -p /home/user/android/build/universal5422-bin
$ mkdir -p /home/user/android/build/universal5422-bin/modules
$ cp /home/user/android/build/universal5422/arch/arm/boot/zImage /home/user/android/build/universal5422-bin/
$ find /home/user/android/build/universal5422/ -type f -name *.ko -exec cp {} /home/user/android/build/universal5422-bin/modules/ \;
```

Your kernel ready in /home/user/android/build/universal5422-bin and all kernel modules in /home/user/android/build/universal5422-bin/modules


- **PART II: Creating a boot image**

Extended version of the Android mkbootimg tool for Exynos 5422 available at [github.com/kumajaya][]

* Copy a stock boot image to /home/user/android/build/universal5422-bin and then unpack it:
```
$ cd /home/user/android/build/universal5422-bin
$ mkdir boot
$ unpackbootimg -i boot.img -o boot
```

* Repack a new boot image:
```
$ mkdtbhbootimg --kernel zImage --ramdisk boot/boot.img-ramdisk.gz --dt_dir /home/user/android/build/universal5422/arch/arm/boot/dts/ -o boot-new.img
```


**DISCLAIMER:**

**USE THIS INSTRUCTIONS AT YOUR OWN RISK, I DON'T OWN SAMSUNG GALAXY 5 EXYNOS 5422 FOR TESTING**


Ketut P. Kumajaya, June 2014

 [github.com/kumajaya]: https://github.com/kumajaya/mkdtbhbootimg
