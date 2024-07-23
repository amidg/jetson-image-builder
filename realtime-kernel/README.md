# Rebuild L4T Linux kernel with PREEMPT_RT (real-time) patch
This page allows to rebuild the kernel for the NVIDIA Jetson in order to include PREEMPT_RT patches.

### How to build?
Build instructions are provided below. Ideally build on Apple Silicon machines in order to get the best performance because cross compilation on amd64 is slow:
```
docker build -t xavier-rt-kernel:latest -f Dockerfile.xavier .
docker run -it -v $(pwd):/app xavier-rt-kernel

docker run --rm -v $(pwd):/host xavier-rt-kernel:32.2.1 \
  cp /Linux_for_Tegra/kernel/kernel_supplements.tbz2 \
     /Linux_for_Tegra/bootloader/payloads_t19x/bl_update_payload \
     /Linux_for_Tegra/bootloader/payloads_t19x/bl_update_payload_default \
       /host
```

### How to update?
In your folder you will have the following files:
- `bl_update_payload` - image for OTA update containing real-time kernel
- `bl_update_payload_default` - image for OTA update containing default kernel
- `kernel_supplements.tbz2` - archive containing kernel modules. Should be extracted to `/lib/modules`

Usually to update Xavier you need to copy `bl_update_payload` to `/opt/ota_package` and execute `nv_update_engine --install`. `kernel_supplements.tbz2` should be extracted to `/lib/modules`.

### Documentation:
[NVIDIA OTA updates documentation.](https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%2520Linux%2520Driver%2520Package%2520Development%2520Guide%2Fbootloader_update.html%23wwconnect_header)
