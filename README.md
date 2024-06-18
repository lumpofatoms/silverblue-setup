# Silverblue-Setup

## NVIDIA graphics driver
**rpmfusion NVIDIA:**  
`sudo rpm-ostree install akmod-nvidia xorg-x11-drv-nvidia-cuda`  
`sudo rpm-ostree kargs --append=rd.driver.blacklist=nouveau --append=modprobe.blacklist=nouveau --append=nvidia-drm.modeset=1`  
`sudo dnf install nvidia-vaapi-driver libva-utils vdpauinfo` (Hardware rendering)

_Reference_:  
[RPMFusion NVIDIA](https://rpmfusion.org/Howto/NVIDIA#OSTree_.28Silverblue.2FKinoite.2Fetc.29)

***

## Hard drive / SSD
**Enable trim on luks SSD:**  
`sudo cryptsetup --allow-discards --persistent refresh /dev/mapper/luks*`  

_References_:  
[Fedoraproject.org Forum post](https://discussion.fedoraproject.org/t/i-need-your-best-performance-tips/103391/12)  
[Lurkerlabs - Fedora Silverblue Ultimate Post Install Guide (alternative method)](https://lurkerlabs.com/fedora-silverblue-ultimate-post-install-guide/)

***

## VSCode / Dev environment  
**podman/vscode:**  
`systemctl --user enable --now podman.socket`      
`flatpak override --user --filesystem=xdg-run/podman com.visualstudio.code`  

**NVIDIA toolkit:**  
`curl -s -L https://nvidia.github.io/libnvidia-container/stable/rpm/nvidia-container-toolkit.repo | \ sudo tee /etc/yum.repos.d/nvidia-container-toolkit.repo`  
`sudo rpm-ostree install -y nvidia-container-toolkit`  
_Reference_: [NVIDIA Wiki](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

`sudo nvidia-ctk cdi generate --output=/etc/cdi/nvidia.yaml`  
`nvidia-ctk cdi list` (optional)  
`podman run --rm --device nvidia.com/gpu=all --security-opt=label=disable ubuntu nvidia-smi -L` (optional)  
`podman run --rm --device nvidia.com/gpu=all --security-opt=label=disable nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark`  
_Reference_: [NVIDIA Wiki](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/cdi-support.html)  

***

## Wayland  
**vscode:**  
`flatpak override --user --socket=wayland com.visualstudio.code`  
`flatpak override --user --env=ELECTRON_OZONE_PLATFORM_HINT=wayland com.visualstudio.code`  
_(Might not be needed anymore after update to NVIDIA driver v555)_

***

## Other  
**tresorit:**  
`chmod u+x tresorit_installer.run`   
`sh tresorit_installer.run`  

**aptX bluetooth codec**:  
`rpm-ostree install pipewire-codec-aptx`  

**ffmpeg codecs:**    
`rpm-ostree override remove libavcodec-free libavfilter-free libavformat-free libavutil-free libpostproc-free libswresample-free libswscale-free ffmpeg-free libavdevice-free --install ffmpeg`  

**flathub:**
Re-install flathub-flatpaks for apps that were installed from fedora repo
`flatpak install --reinstall flathub $(flatpak list --app-runtime=org.fedoraproject.Platform --columns=application | tail -n +1 )`
Remove fedora repo
`flatpak remote-delete fedora`

**Firefox:**  
[Arkenfox](https://github.com/arkenfox/user.js/wiki/4.1-Extensions)  
[Mullvad DNS resolver](https://mullvad.net/en/help/dns-over-https-and-dns-over-tls)

**Gnome Extensions:**
- Extension manager  
	- dash-to-dock  
	- paperwm

**Prevent Super+. from inserting e instead of action:**  
`ibus-setup` -> Under emoji tab, clear 'Emoji annotation'  
_Reference_: [StackOverflow](https://stackoverflow.com/questions/71997823/ctrl-dot-makes-e-appear-instead-of-showing-suggestions-in-vscode-on-gnome)
