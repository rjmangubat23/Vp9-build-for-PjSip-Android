# How to build VP9 codec for android and pjsip

This is a quick guide for building VP9 codec for android and PJSIP

Note: I am using mac OS 


First, Acquire the VP9 codec using these commands via your terminal:

    git clone https://chromium.googlesource.com/webm/libvpx
    cd libvpx
    git co master    ## likely unnecessary
    ./configure --target=armv7-android-gcc --sdk-path=<path of your NDK> --disable-examples --disable-runtime-cpu-detect
    --disable-vp8-encoder --disable-vp8-decoder --disable-examples --enable-static --enable-pic --enable-vp9 
    --disable-runtime-cpu-detect --disable-vp8 --disable-neon
    make
If you see a libvpx.a file inside your folder after make then you are good to go. 

After that, download the latest PJSIP source via http://www.pjsip.org/download.htm, for this guide we will be using version 2.5.5 
Note: I prefer to use the tar.bz2 file rather than the zip file but depends on your preference.

Then, Unzip the PJSIP source, if you are using the tar.bz2 file like me, unzip it via this command on your terminal:
    
    tar zxf <Path of your PJSIP source>
    
After that, take your libvpx.a file which was previously built on your VP9 source and copy it and paste it on the <third party> libs folder of your PJSIP source which can be found on this path: /(Path of your PJSIP source)/third_party/lib

Next, Go to the build.mak.in file of your PJSIP source, which can be found in /(Path of your PJSIP source)/build.mak.in 
Open it with whichsoever notepad you use(I use atom), goto line 279 (which it usually is) or look for via search, "@LIBS@" which is in the export APP_LDLIBS command and paste "-lvpx" which will make your makefile locate for the VP9 codec library via the third_party libraries 
linked to pjsip. 

Your build.mak.in file, for its export APP_LDLIBS command must look like this: 

        export APP_LDLIBS := $(PJSUA_LIB_LDLIB) \
        $(PJSIP_UA_LDLIB) \
        $(PJSIP_SIMPLE_LDLIB) \
        $(PJSIP_LDLIB) \
        $(PJMEDIA_CODEC_LDLIB) \
        $(PJMEDIA_LDLIB) \
        $(PJMEDIA_VIDEODEV_LDLIB) \
        $(PJMEDIA_AUDIODEV_LDLIB) \
        $(PJMEDIA_LDLIB) \
        $(PJNATH_LDLIB) \
        $(PJLIB_UTIL_LDLIB) \
        $(APP_THIRD_PARTY_LIBS)\
        $(APP_THIRD_PARTY_EXT)\
        $(PJLIB_LDLIB) \
        @LIBS@ -lvpx 

When that is finished build your PJSIP via these commands:

      export ANDROID_NDK_ROOT=<Path of your NDK>
      export CFLAGS="-O2 -DNDEBUG"
      cd <Path of your PJSIP source>
      ./configure-android  --disable-silk --disable-speex-codec --disable-gsm-codec --disable-l16-codec --disable-speex-aec --disable-          g722-codec --disable-ilbc-codec --disable-g711-codec --disable-g729-codec --disable-floating-point 
      make dep && make clean && make
      
After that you are all finished, If you see "-lvpx" on your terminal screen printed and If the build has no errors after make command 
then you are all good to go! 


If this has helped you in anyway feel free to share this to other people who are having problems on this. Thank you!

If you have any questions, feel free to email me: mangubatrj@gmail.com

