# this is the first version
This project is to turn a Raspberry PI as an audio device. The device is able to do 2 functions: as a sound collector and as a test-tone generato.

Table of Content
	1. Configuration Instructions
	2. Installation Instructions
	3. Operating Instructions
	4. File list (Manifest)
	5. Copyright / Copyleft
	6. Contact Information
	7. Credits and Acknowledges

1. Configuration Instructions 

	The application is running on Raspberry PI 3 mode B. This PI should come with a USB sound card, a network connection, a microphone.
	
	1.1 Configure USB sound card as default audio device

		1.1.1	RPi onboard sound card doesn’t have microphone interface. We have to change the default audio device to be USB sound card. 
		1.1.2	Boot up RPi, and apply the USB sound card. Use “lsusb” command to check if your USB sound card is mounted:
			pi@raspberrypi:~ $ lsusb
			Bus 001 Device 004: ID 0d8c:000c C-Media Electronics, Inc. Audio Adapter
			Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter
			Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp.
			Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
		1.1.3	Use “sudo nano /etc/asound.conf”command and put following content to the file:
			pcm.!default {
			  type plug
			  slave {
				pcm "hw:1,0"
			  }
			}
			ctl.!default {
				type hw
				card 1
			}
		1.1.4.	Go to your home directory. Use “nano .asoundrc” command and put the same content to the file.

		1.1.5.	Run “alsamixer” you should be able to see that USB sound card is the default audio device. For a more sensitive sound detection, it is better to maximize the volume of “Mic”.
	1.2 Fix the Bug of arecord
		
		1.2.1	The newest version of Raspbian (a.k.a. Jessie) comes with a new version of alsa-utils (1.0.28), which has a bug while recording: it doesn’t stop even a '—duration' switch is given, and generates lots of useless audio files. To fix this problem, we have to downgrade alsa-util to an earlier version (1.0.25).

		1.2.2	Use “sudo nano /etc/apt/sources.list” command and add the last line:
			deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
			# Uncomment line below then 'apt-get update' to enable 'apt-get source'
			#deb-src http://archive.raspbian.org/raspbian/ jessie main contrib non-free rpi
			deb http://mirrordirector.raspbian.org/raspbian/ wheezy main contrib non-free rpi
		1.2.3	Run “sudo apt-get update”

		1.2.4	Run “sudo aptitude versions alsa-utils” to check if version 1.0.25 of alsa-util is available:
		
			pi@raspberrypi:~ $ sudo aptitude versions alsa-utils
			Package alsa-utils:
			i   1.0.25-4                                    oldstable                                 500
			p   1.0.28-1                                    stable                                    500

		1.2.5	Run “sudo apt-get install alsa-utils=1.0.25-4” to downgrade.

		1.2.6	Reboot (if necessary)

		1.2.7	Run “arecord -r44100 -c1 -f S16_LE -d5 test.wav” to test that your microphone is working. You should see a “test.wav” file in the current folder.

		1.2.8	Put earphone on the USB sound card. Run “aplay test.wav” to check that your recording is okay.

	1.3 Install libcurl
		1.	First use command "ls /usr/include/curl" or "ls /usr/include/arm-linux-gnueabihf/curl" to identify that libcurl library is installed.

		2.	If the folder doesn’t exist. Run “sudo apt-get update” to update the application list.

		3.	Run “sudo apt-get install libcurl3” to install the libcurl3.

		4.	Run “sudo apt-get install libcurl4-openssl-dev” to install the development API of libcurl4.

2. Installation Instructions
	
	To download these source code, you need to write a command " git clone https://github.com/0984647853/appdev.git ". 
	Now your source code is ready for executing. Just following the " Operating Instructions ".

3. Operating Instructions

This device able to do 2 functions:
 If you want it as:
	A sound collector, do the following instruction: 
		+ Use command " make " to compile all the file, make sure that all the file *.c is converted into *.o
		+ Then write " ./sound.out ". You will see the graph which is representing the sound intensity. 
	A test-tone generator, do the following instruction:
		+ Also write " make " to compile.
		+ Then, write " ./sound.out xxx " you have to give the amount of frequency between 30 - 16000.
		+ The device will ask you " How many channels do you want ? " So choose 1 for 1 speaker or 2 for 2 speakers.
		+ Then you select the duration for the test-tone. ( 1 - 10 seconds)
		+ You will able to listen the sound when you write " aplay testTone.wav " but make sure that you'd seen the line " Test tone is generated ! " before. 
4. File list (manifest)

	4.1 main.c
		The file which is the main file containning the functions of other .c file to run the program.
	4.2 sound.c (included header files sound.h, comm.h, and screen.h)
		Display information of WAVE file format and display .wav file data.
	4.3 screen.c (included header files screen.h)
		Display .wav file data in bar-diagram.
	4.4 comm.c (included header file comm.h)
		Send data to server.
	4.5 makefile
		Compile all the file.
	
5. Copyright / Copyleft

	You have full access to copyright this project.

6. Contact Information
	Nguyen Dang Toan
	Email: nguyendangtoan2305@gmail.com
	Study at: VAMK Vaasa.
7. Credits and Acknowledges
	Thanks to Mr Gao Chao and his help and guidance.
	
