+++
title = "Audacity with AAC Support on Linux"
date = 2023-12-29
[extra]
abstract = "How to teach some world-class FOSS (Free and Open Source Software) a new trick"
+++

## The Story

I recently had the good fortune of becoming part of the team at [Radio Free Montclair](https://www.radiofreemontclair.org) (RFM) and have decided to come out of "DJ retirement" to start a new show. These days I prefer to use FOSS (Free Open Source Software) to do my internet radio DJing, and I use [Mixxx](https://mixxx.org) for actual DJing and [Audacity](https://www.audacityteam.org) for post-production.

RFM uses an internet-radio-station-as-a-service platform called [Radio.co](https://radio.co/) under the hood. This platform accepts two file formats: MP3 and AAC. The latter is the higher quality option among the two, and being in the paradoxical postion of an audiophile internet radio DJ, my goal is to cram as much quality as possible into bitrate-limited audio streams. So, AAC is my format of choice.

However, AAC is a "non-free" (i.e. proprietary) format, so the stock Audacity available as a pre-built binary application does not include support for this format. I was able to do a custom build of some underlying software called `ffmpeg` to add AAC support to it, and this has allowed me to export in this format for my radio show!

Doing so turned out to be non-trivial, so I'm writing this post in the hopes that it may help you as well. Are you a technologically adventurous internet radio DJ? I encourage you to take this same approach.

I started with the official custom build instructions for Ubuntu-like systems available [here](https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu) and had to make a tweak based on this [SO answer](https://stackoverflow.com/a/36745455) to enable Audacity integration. 


Here are the commands as tweaked, that allowed me to successfully export AAC from Audacity on an Ubuntu-like system.


## Installing Dependencies

```
sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libass-dev \
  libfreetype6-dev \
  libgnutls28-dev \
  libmp3lame-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  meson \
  ninja-build \
  pkg-config \
  texinfo \
  wget \
  yasm \
  zlib1g-dev \
  libunistring-dev \ 
  libaom-dev \
  libdav1d-dev
```


## Building `ffmpeg` with AAC Support

```
mkdir -p ~/ffmpeg_sources ~/bin && \
cd ~/ffmpeg_sources && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --ld="g++" \
  --bindir="$HOME/bin" \
  --enable-shared \
  --disable-static \
  --enable-gpl \
  --enable-gnutls \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libdav1d \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree && \
PATH="$HOME/bin:$PATH" make -j$(nproc) && \
make -j$(nproc) install && \
hash -r
```

## Finishing Up

Now you have a custom `ffmpeg` build along with the necessary source files in your home directory. 

From here, I strongly recommend using the AppImage of Audacity to get this working (available from the Audacity website linked above). Download and run the AppImage, then go to `Edit>Preferences>Libraries`. Here, click on the "Locate" button and it will request that you choose the location of a file associated with `ffmpeg` which is called `libavformat.so`. After doing the custom build with the tweaks described above, this file should be located at `~/ffmpeg_sources/ffmpeg/libavformat/libavformat.so` so input that location, and from there AAC support should be enabled. 

That about wraps it up. Happy exporting! 
