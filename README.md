# dotOS - Kali 2 like a real boy
dotOS is a custom Kali 2 installation geared towards hackers who feel more comfortable with a keyboard and don't appreciate the delays introduced by using a mouse all the time.   
Build with Kali 2, i3wm, oh-my-zsh, ptf, vimperator (firefox), conky and a few extra things here and there that make [my](https://twitter.com/kussic) hacking more streamlined.

![Just a screenshot](screenshot.png "A screenshot")

>Step by step instructions below. Your milleage may vary since things do change every now and again. If some instruction doesn't work fix it and then let me know. _I'm not your customer support  desk!_

1. Build your custom Kali 2 i3wm iso   
    From an existing Kali installation run the following commands:
	```
	apt-get install curl git live-build cdebootstrap
	git clone git://git.kali.org/live-build-config.git
	cd live-build-config
	```
	At this point you can customise the contents of the distribution, by default it's the full Kali install with everything included.
	If you're okay with this skip the next step and go straight to the __"./build.sh [...]"__ command below.
	I personally hate bloat so I tweak __"kali-config/variant-i3wm/package-lists/kali.list.chroot"__ to only install the Top 10 by default:
	```
	# You always want those
	kali-linux
	kali-desktop-live

	# Kali applications

	# You can customize the set of Kali applications to install
	# (-full is the default, -all is absolutely everything, the rest
	# corresponds to various subsets)
	# kali-linux-full
	# kali-linux-all
	# kali-linux-sdr
	# kali-linux-gpu
	# kali-linux-wireless
	# kali-linux-web
	# kali-linux-forensic
	# kali-linux-voip
	# kali-linux-pwtools
	kali-linux-top10
	# kali-linux-rfid

	# Graphical desktop
	kali-desktop-common
	xorg
	dmenu
	conky
	i3
	```
	I deal with everything else during post installation. This makes a leaner disk and a smaller install size and time.

			
	Run the build script:
	`./build.sh --distribution sana --variant i3wm --verbose`

	>NOTE: this will take its sweet time, at the end you will have an .iso file if everything went right!

2. Install Kali 2   
   Just follow the normal steps to install Kali in your VM and install the right vmtools.

   Virtualisation | Command
   ---------------|---------
   VirtualBox     | apt-get install virtualbox-guest-utils
   VMware         | _Use the vmware tools that come with Player/Workstation_

3. Add Penetration Testing Framework   
   The following will install my own fork of PTF, which has some additional tools removed a number of tools I feel are better off if you do it the "kali" way (e.g. metasploit).   
   Install, run it and just add the tools you like, as needed…
   ```
   cd /opt/
   git clone https://github.com/kussic/ptf.git
   cd ptf
   ./ptf
   ```	
		
		
4. Add extra tools
	``` 
	#Add i386 architecture support and wine32
	dpkg --add-architecture i386 && apt-get update && apt-get install wine32

	#Install some most needed extra tools
	apt-get install arp-scan passing-the-hash sslyze netdiscover mono-runtime proxychains freerdp tshark responder python-openssl wmis hashid python-pyside eyewitness mitmproxy netwox ike-scan

	#Install the awesomeness that is impacket
	pip install pyasn1 && pip install impacket

	#Get your local clone
	cd ~ && git clone https://github.com/kussic/dotOS.git
	```

5. Install Oh-my-zsh   
    `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
	
	_Optional: append the "DO NOT PANIC" logo to .zshrc_ 
	```
	mv ~/dotOS/dotfiles/dotos-bin ~/.dotos-bin
	echo "~/.dotos-bin/obey2" >> ~/.zshrc
	```

6. Install the custom i3wm dotfiles
	```
	mv ~/i3 ~/i3-bak
	mv ~/dotOS/dotfiles/i3 ~/.i3
	```

7. Firefox with Vimperator (Optional but extra-leet-awesomesause)
	1. Download the right version of firefox from [here] (https://www.mozilla.org/en-US/firefox/all/) (look for, "Linux x64")
	2. Extract under `/opt/`
	3. Install vimperator plugin from within Firefox from [here] (https://addons.mozilla.org/en-US/firefox/addon/vimperator)
	4. Install Stylish plugin from within Firefox from [here] (https://addons.mozilla.org/en-US/firefox/addon/stylish)
	5. Apply the "~/dotOS/dotfiles/vimperator/firefox-css" to Stylish

##Credits
* Obey2 based on the idea of Archbey2 by Mr Green
* i3wm scripts based on the work of https://github.com/strang3quark
* PTF is forked from https://github.com/trustedsec/ptf and made by @HackingDave
* Vimperator firefox CSS by http://twily.info/
