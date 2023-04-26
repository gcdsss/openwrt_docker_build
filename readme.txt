Usage MacOS

hdiutil create -size 20g -fs "Case-sensitive HFS+" -volname OpenWrt OpenWrt.dmg
hdiutil attach OpenWrt.dmg

docker buildx build --platform=linux/amd64 -t guihua/openwrt_build .

docker run --rm -it -v $(pwd):/openwrt_build -w /openwrt_build guihua/openwrt_build /bin/bash

git clone https://git.openwrt.org/openwrt/openwrt.git
cd openwrt
git pull
 
# Select a specific code revision
git branch -a
git tag
git checkout v21.02.3
 
# Update the feeds
./scripts/feeds update -a
./scripts/feeds install -a
 
# Configure the firmware image and the kernel
make menuconfig

# Build the firmware image
make -j $(nproc)