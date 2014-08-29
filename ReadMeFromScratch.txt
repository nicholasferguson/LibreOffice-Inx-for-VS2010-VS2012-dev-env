This just a kick off set of instructions.  It's expected to work.  Then you can improve as you see fit.
This is basically for colleagues at an office. 


LibreOffice_cygwin_VS2012_vs2010
Build was on a fresh, new machine.  There were issues with MSBuild on a machine that had
several dev environments already installed on it.  A strategy of correcting those issues by reinstalling NET, SDK and Visual Studio takes hours and my 
sequence didn't work.  I was also using Windows 8.   I also had issues with Norton
AntiVirus which would quarantine files from this build ... without flagging them as a virus,
but a threat...?



So uninstall, reinstall. I finally just bought a new machine O/S Windows 7 Pro, 64 bit

Created dir c:\cygwin64

/cygwin64
	README
	get the setup-x86_64.exe from cygwin.org
	run it by using start.setup.bat
		this bat is configured to download needed packages from cygwin as to build libreoffice.
	I recommend running cygwin setup  twice.
		1. Download to a local directory
			1.1.  Choose a local directory in c:/cygwin64...like 
							  c:/cygwin64/install
		2. Also if you have bandwidth, choose a site like
			2.1   http://mirror.mcs.anl.gov  
			2.2   They have bandwidth on their side
		3. After it downloads, then restart this install and choose
			3.1. Install from local directory.

	Then start up cygwin64 and at prompt type in
	>cpan -i Archive::Zip
	for some reason this perl utility is missing.

	Install ant in c:/cygwin64/opt/apache-ant-1.9.2
	Install junit in c:/cygwin6/opt/junit/share/java/junit-4.4.jar & junit-4.1.1.jar

	Create dir for /opt/lo/bin  ( a 64 bit make will be installed here )
		Put the make.exe here.  then at prompt >chmod +x make.exe

	Add this line to your home directory .bash_profile ( this is the only edit to default .bashrc and .bash_profile
		PATH=/opt/lo/bin:$PATH
	After an initial startup of cygwin64...a .bash_profile will be automatically created for you, in your home directory.

/Do a git from gerrit.libreoffice.org
	ex:  git clone git://gerrit.libreoffice.org/core
	untar via cygwin
	do mv <name of untarred file> master  and put here: /opt/lo/master



/Special_config_filesToReview.
	autogen.input
	config_host.mk

/MS.Installs  ( download their iso's...otherwise install takes too long)

/Instructions
Step 1:
	install cygwin64 as explained above.

Step 2:  Install MS packages.  There is a file in MS.Installs with iso names.

	For all of these packages, its recommended you first downloaded their iso files.
	Just in case, you need to reinstall.  Also downloading iso file then installing is
	faster then installing these packages onto your machine's directories, via web.

	Install NET 3.0,4.0, 4.2 ( not 4.5 is only needed to do builds with VS2013)
		First complete this build, with  2010, then try 2012.  LibreOffice is still working on releasing a 2013 build.
		As far as I can tell, builds are good only for Windows 7 Pro. 

	Install MSBuild v 12
	Install Framework SDKs 7.1 8.0 8.1 ( note 8.2 is for VS2013)
	Install VS 2012 Pro ( Trial version.  Get Is
	Install VS 2012 Update 4
	Install VS 2010 Pro (Trial version)
	Install VS 2010 SP 1.

	For VS2010, download DirectX
		You will need to add DirectX to the config_host.mk path. 
		My implementations eventually required this.
	Install DirectXSDK for a Visual Studio 2010 build.

	install Java JDK 1.8 or a 1.7 32 bit.
	download ant   put into /opt/ant
		ant version is apache-ant-1.9.2
	download junit put into /opt/junit/share/java
		junit version is junit-4.4.jar and junit-4.1.1.jar

	After these microsoft packages are installed, check for any updates.
	Control Panel\System and Security\Windows Update
	Install all.

Step 3:
	start up your cygwin as windows  administrator
	cd /opt/lo/master
	>./autogen.sh
	>/opt/lo/bin/make
		Make will take about four hours.

	Start up cygwin and go to /opt/lo/bin and do chmod+x on the make.exe
Step 3X

	If autogen.sh or make complains that a cygwin lib or whatever is missing.
	run start.setup.bat
	Click on cygwin gui "keep'  That says none of your existing cygwin will be downloaded again
	Then do a search in cygwin gui for items that are missing and download without installation.
	Then repeat this process, but install from local directory. (again choose keep, and search for item that needs to be installed)

Step 4:
	Review autogen.input file
	When you run autogen.sh, it will initially downloaded tar files from LibreOffice.
	After it downloads, kill make.
	Edit your autogen.input file with these notes
	This will prevent further downloadso of these tars.
step 5:

	If there is a core, or fault.  
	Important
	Assuming your make downloaded all tar files.
	Edit your autogen.input and add 
	--disable-external-fetch
	This will prevent make from doing wget for these same files, over and over again.
	do make clean. Then repeat

	./autogen.sh
	make


Step 6:
	I recommend running make 
	>/opt/lo/bin/make > m1.log 2>$1

	Then start up a 2nd cygwin64 instance that will give a view into m1.log file with line numbers.
	cd /home/lo/master
	> tail -f m1.log | perl -pe '$_ = "$. $_"'

Step 7:
	
	Builds for VS2010 might complain that it cannot find header files for DirectX.
	Look in config_host.mk and insure that path to DirecX is included here:

	

	





		