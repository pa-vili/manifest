# Paranoid Android (AOSPA) Calcite Manifest for Xiaomi 11T Pro (`vili`)

This repository provides the local manifest and complete setup guide for building **Paranoid Android (AOSPA) Calcite (Android 14)** for the **Xiaomi 11T Pro (`vili`)** based on the Qualcomm Snapdragon 888 5G (`sm8350` / `lahaina`) platform.

All repositories are sourced directly from the [**pa-vili**](https://github.com/pa-vili) organization.

---

## Method 1: Local Manifest (Recommended)

From the root directory of your AOSPA build tree:

```bash
# 1. Create local manifests directory
mkdir -p .repo/local_manifests

# 2. Download vili local manifest
curl -o .repo/local_manifests/vili.xml https://raw.githubusercontent.com/pa-vili/manifest/calcite/vili.xml

# 3. Sync all repositories
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

---

## Method 2: Manual Clone Setup

If you prefer to manually clone the repositories into your existing source tree, run the following commands from the root of your Android tree:

### 1. Remove conflicting stock/platform packages
```bash
rm -rf hardware/qcom/bootctrl
rm -rf vendor/qcom/opensource/thermal-hal
```

### 2. Clone Vendor Blobs & HAL Repositories
```bash
# QTI QRD Vendor Blobs
git clone https://github.com/pa-vili/vendor_qcom_qrd.git -b uvite vendor/qcom/qrd

# NXP NFC HAL & HIDL
git clone https://github.com/pa-vili/android_vendor_nxp_opensource_halimpl.git -b vauxite-SN100X-legacy vendor/nxp/opensource/halimpl
git clone https://github.com/pa-vili/android_vendor_nxp_opensource_hidlimpl.git -b vauxite-SN100X-legacy vendor/nxp/opensource/hidlimpl

# Qualcomm & Xiaomi Hardware HALs
git clone https://github.com/pa-vili/android_hardware_qcom_audio.git -b calcite-888 vendor/qcom/opensource/audio-hal/primary-hal
git clone https://github.com/pa-vili/android_hardware_qcom_display.git -b calcite-888 hardware/qcom/display
git clone https://github.com/pa-vili/android_hardware_qcom_gps.git -b calcite-legacy-component hardware/qcom/gps
git clone https://github.com/pa-vili/android_hardware_qcom_media.git -b calcite-888 hardware/qcom/media
git clone https://github.com/pa-vili/android_hardware_qcom_thermal.git -b calcite-legacy vendor/qcom/opensource/thermal-hal
git clone https://github.com/pa-vili/android_hardware_xiaomi.git -b calcite hardware/xiaomi
```

### 3. Clone Device & Proprietary Trees
```bash
git clone https://github.com/pa-vili/device_xiaomi_vili.git -b calcite device/xiaomi/vili
git clone https://github.com/pa-vili/device_xiaomi_sm8350-common.git -b calcite device/xiaomi/sm8350-common
git clone https://github.com/pa-vili/device_xiaomi_vili-kernel.git -b calcite device/xiaomi/vili-kernel
git clone https://github.com/pa-vili/proprietary_vendor_xiaomi_vili.git -b calcite vendor/xiaomi/vili
git clone https://github.com/pa-vili/proprietary_vendor_xiaomi_sm8350-common.git -b calcite vendor/xiaomi/sm8350-common
```

---

## Building the ROM

Once all repositories are synced or cloned, start the build process:

```bash
# 1. Initialize environment
. build/envsetup.sh

# 2. Lunch AOSPA vili target
lunch aospa_vili-userdebug

# 3. Compile ROM
m bacon
```

---

## Repository Mapping Summary

| Target Location in Tree | Remote Repository (`pa-vili`) | Branch |
| :--- | :--- | :--- |
| `vendor/qcom/qrd` | `pa-vili/vendor_qcom_qrd` | `uvite` |
| `vendor/nxp/opensource/halimpl` | `pa-vili/android_vendor_nxp_opensource_halimpl` | `vauxite-SN100X-legacy` |
| `vendor/nxp/opensource/hidlimpl` | `pa-vili/android_vendor_nxp_opensource_hidlimpl` | `vauxite-SN100X-legacy` |
| `vendor/qcom/opensource/audio-hal/primary-hal` | `pa-vili/android_hardware_qcom_audio` | `calcite-888` |
| `hardware/qcom/display` | `pa-vili/android_hardware_qcom_display` | `calcite-888` |
| `hardware/qcom/gps` | `pa-vili/android_hardware_qcom_gps` | `calcite-legacy-component` |
| `hardware/qcom/media` | `pa-vili/android_hardware_qcom_media` | `calcite-888` |
| `vendor/qcom/opensource/thermal-hal` | `pa-vili/android_hardware_qcom_thermal` | `calcite-legacy` |
| `hardware/xiaomi` | `pa-vili/android_hardware_xiaomi` | `calcite` |
| `device/xiaomi/vili` | `pa-vili/device_xiaomi_vili` | `calcite` |
| `device/xiaomi/sm8350-common` | `pa-vili/device_xiaomi_sm8350-common` | `calcite` |
| `device/xiaomi/vili-kernel` | `pa-vili/device_xiaomi_vili-kernel` | `calcite` |
| `vendor/xiaomi/vili` | `pa-vili/proprietary_vendor_xiaomi_vili` | `calcite` |
| `vendor/xiaomi/sm8350-common` | `pa-vili/proprietary_vendor_xiaomi_sm8350-common` | `calcite` |
