# pjsip-opus-codec

The aim of this repository is enabling only opus codec for the ojsip - pjproject.

It is tested on both Windows Visual Studio 2022 and Ubuntu 22.04 LTS

# Steps for Windows

- First , clone the pjsip/pjproject

```bash
git clone https://github.com/pjsip/pjproject.git
```

- Download the opus source code from here : https://www.opus-codec.org/downloads/
- Extract the zipped files and find win32/VS2015 folder. Open the solution with Visual Studio. Build the solution with proper build settings with settings that should be same with pjsua. (Relase x64 is used).
- If opus.lib is generated after successful build, Open pjproject with Visual Studio. You can close the previous one that is used to build opus.
- Retarget solution, set pjsua as startup project and retarget the pjsua project. Set the same settings with the opus build. (Release x64 etc)
- Right click solution, Click Add existing project and select the opus.vcxproj file that is inside of the win32/VS2015 folder that is downloaded before.
- Add opus project dependency to pjsua project by right clicking to pjsua project, then click build dependencies, project dependecies and select the opus.
- Copy the files under opus/include to inside of the pjproject/opus. If there is no opus folder inside of the pjproject, create it , then copy files.
- Right click pjsua project, select properties and in C/C++ section, add path to files that copied in the previous step. Path should be something like that : **C:\Users\Bursiyer\Desktop\Frameworks\opus-1.4.tar\opus-1.4\include**
- In the properties→ Linker→ General , add an Additional Library Directory. Add full path for opus.lib file that is generated in opus build.
- In Linker → Input section add an Additional Dependecy. Add full path for opus.lib file that is generated in opus build.
- Then modify the line end of pjmedia/src/pjmedia-codec/opus.c

```c
#   pragma comment(lib, "libopus.a")
```

to

```c
#   pragma comment(lib, "opus.lib")
```

- If dll is generated,  then change opus.lib to opus.dll.
- Set your config_site.h like that :

```c
#include <pj/config_site_sample.h>
#define PJMEDIA_HAS_OPUS_CODEC 1
#define PJMEDIA_HAS_SPEEX 0
#define PJMEDIA_HAS_PASSTHROUGH_CODEC_PCMU 0
#define PJMEDIA_HAS_PASSTHROUGH_CODEC_PCMA 0
#define PJMEDIA_HAS_G711_CODEC           0
#define PJMEDIA_HAS_SPEEX_AEC            0
#define PJMEDIA_HAS_L16_CODEC            0
#define PJMEDIA_HAS_GSM_CODEC            0
#define PJMEDIA_HAS_SPEEX_CODEC          0
#define PJMEDIA_HAS_ILBC_CODEC           0
#define PJMEDIA_HAS_G722_CODEC           0
#define PJMEDIA_HAS_G7221_CODEC          0
#define PJMEDIA_HAS_OPENCORE_AMRNB_CODEC 0
```

# Steps for Unix

- First , clone the pjsip/pjproject

```bash
git clone https://github.com/pjsip/pjproject.git
```

- Download the opus source code from here : https://www.opus-codec.org/downloads/
- Extract the zipped files and run configure and make

```bash
./configure && make && sudo make install
```

- Set your config_site.h like that :

```c
#include <pj/config_site_sample.h>
#define PJMEDIA_HAS_OPUS_CODEC 1
#define PJMEDIA_HAS_SPEEX 0
#define PJMEDIA_HAS_PASSTHROUGH_CODEC_PCMU 0
#define PJMEDIA_HAS_PASSTHROUGH_CODEC_PCMA 0
#define PJMEDIA_HAS_G711_CODEC           0
#define PJMEDIA_HAS_SPEEX_AEC            0
#define PJMEDIA_HAS_L16_CODEC            0
#define PJMEDIA_HAS_GSM_CODEC            0
#define PJMEDIA_HAS_SPEEX_CODEC          0
#define PJMEDIA_HAS_ILBC_CODEC           0
#define PJMEDIA_HAS_G722_CODEC           0
#define PJMEDIA_HAS_G7221_CODEC          0
#define PJMEDIA_HAS_OPENCORE_AMRNB_CODEC 0
```

- Install libraries to make sure that sound is coming properly :

```bash
sudo apt-get install portaudio19-dev libportaudio2 pulseaudio alsa-utils alsa-base alsa-tools libasound2-plugins libasound2 libasound2-dev binutils binutils-dev libasound-dev
```

- Run proper pjsip configure, make dep and make. If you have other options like openssl, dont forget to add them. Example :

```bash
./configure && make dep -j8 && sudo make -j8 && sudo make install -j8
```

- Run the pjsip-apps/bin/pjsua sample with these options :

```bash
--capture-dev=-1 --playback-dev=-1
```

# Changing Default Settings of OPUS

- Default settings for opus codec are for bit rate 48000 and for channel count 2. If you want to change them, go to opus.c in the pjmedia/src/pjmedia-codec and find factory_enum_codecs function. Set the necesarry fields for your needs. For example :

