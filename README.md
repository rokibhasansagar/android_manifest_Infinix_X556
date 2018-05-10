## Building Manual

#### Installing Java and Tools
Add Java repository if not installed/added:
```shell
sudo apt-add-repository ppa:openjdk-r/ppa
sudo apt-get update -y
sudo apt-get install openjdk-8-jdk -y
```
Then install all other needed tools:
```shell
sudo apt-get update -y && sudo apt-get install -y git-core gnupg flex bison gperf libsdl1.2-dev libesd0-dev libwxgtk2.8-dev squashfs-tools build-essential zip curl libncurses5-dev zlib1g-dev openjdk-8-jre openjdk-8-jdk pngcrush schedtool libxml2 libxml2-utils xsltproc lzop libc6-dev schedtool g++-multilib lib32z1-dev lib32ncurses5-dev lib32readline-gplv2-dev gcc-multilib maven tmux screen w3m ncftp
```
#### Downloading Source
Installing repo tools:
```shell
mkdir ~/bin
PATH=~/bin:$PATH
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```
Create build base directory (it can be anything & everything, we will use AOSP as example):
```shell
mkdir ~/AOSP
cd ~/AOSP
```
Configure Your GitHub Account Authorization:
```shell
git config --global user.email "aaa@bbbbbb.com" (Put your github email address) 
git config --global user.name "NAME" (Put your github username)
```
#### Initialize Repo:
```shell
repo init -u (GitHub link to the preferred source manifest) -b (Branch to build) --depth=1 --groups=all,-notdefault,-device,-mips,-x86,-darwin,-exynos5
```
For _example_, if I want to build Reurrection Remix, I'll use:
```shell
repo init -u https://github.com/ResurrectionRemix/platform_manifest.git -b nougat --depth=1 --groups=all,-notdefault,-device,-mips,-x86,-darwin,-exynos5
```
Install local manifest for fix
```shell
curl --create-dirs -L -o .repo/local_manifests/manifest_X556.xml -O -L https://raw.githubusercontent.com/rokibhasansagar/android_manifest_Infinix_X556/master/manifest_X556.xml
```
After Initialization, Sync (it may require some hours to download everything:
```shell
repo sync -c -f --force-sync --no-clone-bundle
```
#### Building ROM from Source
Move to Repo folder (in our case, AOSP):
```shell
cd ~/AOSP
```
Building commands:
```shell
cd device/Infinix/X556/patches_mtk && . apply-patches.sh
```
(if patch fails, do it manually or build won't work)

```shell
export USE_CCACHE=1
export ANDROID_JACK_VM_ARGS="-Xmx4096m -Xms512m -Dfile.encoding=UTF-8 -XX:+TieredCompilation"
./prebuilts/sdk/tools/jack-admin kill-server
./prebuilts/sdk/tools/jack-admin start-server
. build/envsetup.sh && brunch X556
```
