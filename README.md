# GKI Custom

Enable LXC, Docker support for GKI Kernel


### Notice
Currently only supports android12-5.10


### Build

#### Build with action

Fork repo and run new workflow

#### Manual Build

Sync Sources</br>
Reference KernelSU [how to build](https://kernelsu.org/guide/how-to-build.html)

```bash
mkdir android-kernel; cd android-kernel
repo init --depth 1 -u https://android.googlesource.com/kernel/manifest -b [BRANCH]
repo sync
```

#### Clone Repo

```bash
git clone https://github.com/TapetalArray/gki-custom
```

#### Apply Change

Usually we need to add these options to meet the basic requirements of docker. But for GKI kernel it is a little different, check this [diff](https://github.com/TapetalArray/gki-custom/blob/main/docker-config.diff)

```txt
CONFIG_SYSVIPC=y
CONFIG_UTS_NS=y
CONFIG_PID_NS=y
CONFIG_IPC_NS=y
CONFIG_USER_NS=y
CONFIG_NET_NS=y
CONFIG_CGROUP_DEVICE=y
CONFIG_CGROUP_FREEZER=y
```

You can also use this </br>
**Only available android12-5.10**

```bash
cp ./gki-custom/config/gki_defconfig-android12-5.10 ./android-kernel/common/arch/arm64/configs/gki_defconfig
```

Patch

```bash
git apply -v ../../gki-custom/patches/android12-5.10/*.patch
```

Save

```bash
BUILD_CONFIG=common/build.config.gki.aarch64 build/config.sh savedefconfig
```

#### Build

```bash
LTO=thin BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh
```

#### Create Image

Download [gki-release-builds](https://source.android.com/docs/core/architecture/kernel/gki-release-builds).

```bash
./tools/mkbootimg/unpack_bootimg.py --boot_img path/to/img
./tools/mkbootimg/mkbootimg.py --header_version 4 --kernel path/to/Image --ramdisk path/to/ramdisk --os_version [OS_VERSION] --os_patch_level [OS_PATCH_LEVEL] -o path/to/img
```


### Contribute

Support for other release GKI


# Credits

[lateautumn233](https://github.com/lateautumn233)
[TapetalArray](https://t.me/TapetalArray)
