# Device Tree for Xiaomi Black Shark

## Compile

First checkout minimal twrp with omnirom tree:

```
repo init -u git://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-9.0
repo sync
```

Then add these projects to .repo/manifest.xml:

```xml
<project path="device/blackshark/shark" name="JengaMasterG/android_device_blackshark_shark" remote="github" revision="android-9.0" />
```

To make all works you need to modify the buildinfo.sh in build/tools
```echo "ro.build.version.release=$PLATFORM_VERSION"
echo "ro.build.version.security_patch=$PLATFORM_SECURITY_PATCH"
```
to
```echo "ro.build.version.release_orig=$PLATFORM_VERSION"
echo "ro.build.version.security_patch_orig=$PLATFORM_SECURITY_PATCH"
```
And you need to increase the PLATFORM_VERSION to 16.1.0 in build/core/version_defaults.mk to override Google's anti-rollback features

Finally execute these:

```
. build/envsetup.sh
export ALLOW_MISSING_DEPENDENCIES=true # Only if you use minimal twrp tree.
lunch omni_enchilada-eng 
mka adbd recoveryimage 
```

To test it:

```
fastboot boot out/target/product/shark/recovery.img
```

Thanks to @Mauronofrio for the Black Shark device files!
