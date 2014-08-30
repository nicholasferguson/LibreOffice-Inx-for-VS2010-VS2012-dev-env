This set of instructions originated during August 2014, using LibreOffice master.  
That is a dev environment.
I presume these instructions should apply to any LibreOffice version released as of Aug 2014

This is a 'begin development' set of instructions.  It's expected to work.  
Then you can improve as you see fit.

This env will first result in project files for Visual Studio 2012, having Update #4
There are instructions for generating project files for Visual Studio 2010, having Sp #1
In Visual Studio,  when you open LibreOffice.sln, you will find 338 projects

On a new machine, it should take about 4 hours to install microsoft applications and cygwin
and do a git for libreoffice core.

Note:  LibreOffice source tar file  for core is sole tar needed at this stage.
	   If you decide to work with tar files, and word 'core' is not in its name, then it's tar file without word of 'help',
	   'translation' or 'dictionaries'.
	   At this stage, you can skip downloading libreoffice source tar files for 'help', 'translations' and 'dictionaries'.

Then it takes another four to seven hours to do an initial build of LibreOffice.

Length of this time was measured on a 4 core 3.7GHZ with 8GB physical memory, running 
Windows 7 Pro (64bit)

=================
Summary:
=================
	+ LibreOffice is a build using Microsoft's MSBuild, with scripts run via cygwin64
	+ This build uses Boost, and other third party librairies.
	+ This build will initially download from a LibreOffice site, tars of 
	  all 3rd Party tools, such as boost.
	+ This build will then build even boost libs/dlls as required.

	+ Different versions of LibreOffice, have their unique set of identities in their third party 
		tar file-names, such as boost.
	+ This promises that, during a 'make' when that 'make' process downloads third-party tar files 
		from a LibreOffice site, those tar files will work with that LibreOffice version.
		As of Aug 2014, build process downloaded 90 3rd Party tar files, with total size of about 274MB
	
	+ It's important that after this 'make' build process, downloads tar files, that you stop 
		repeated downloads by adding a flag to autogen.input

		--disable-external-fetch

	+ This flag will tell build process to fetch those tars from a local directory, only.  
		Your autogen.input file specifies a local directory:
	
		--with-external-tar=/vboxsvr/tml/lo/src
		
	+ Verify that all of your downloaded 3rdParty tar files from LibreOffice, are in there.  
	    If not, then move them over

	+ Of course, depending on when you interrupt this build process, to prevent 
		further downloads of tar files, if 'build' gets interruped, you will have to restart 
		this build process... 1. run autogen.sh 2. run make

	+ During a build, build will generate tmp files to keep track of its state.
		These tmp file get flagged by Norton Anti-Virus as viruses or threats.
		Even some generated exe's get flagged as 'threats'
	+ Norton will quarantine and delete .... build fails.
	+ On this issue, in googling, there are some complaints about Norton and other anti-viruses.

	+ So to finish a build, I had to un-install Norton.

=================
Special note about Excel VBA and LibreOffice Calc
=================
	Per googling for literature, Excel VBA code is tighthly coupled to Excel's OO Model. 
		Supposing that Excel VBA code interacts with Excel OO.
	LibreOffice Calc has a different OO Model.
		Thus, Excel VBA is not compatible with LibreOffice Calc.
		LibreOffice Calc OO Model and Excel VBA code do not understand each other.
	LibreOffice and its Calc works 100% with LibreOffice Basic and StarBasic.
		
=================		
LibreOffice-Inx-for-VS2010-VS2012-dev-env
=================
Build was on a fresh, new machine.  

Otherwise, there were issues with MSBuild on a machine that had
several dev environments already installed on it.  A strategy of 
correcting those issues  by reinstalling NET,  SDK and Visual Studio 
took hours/days and my sequence didn't work.  I was also using Windows 8.   
I also had issues with Norton AntiVirus which would quarantine files from this build ...
without flagging them as a virus, but a threat...?

So instead of uninstall, reinstall. I bought a new machine with O/S Windows 7 Pro, 64 bit

=========
STEP A - setup cygwin64
=========

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
		
	Create dir vboxsvr and subdirectories for C:\cygwin64\vboxsvr\tml\lo\src
		This directory will store LibreOffice's 90 3rd Party tar files, that will get downloaded
		during 'build' process.
		This directory is referenced in autogen.input
		--with-external-tar=/vboxsvr/tml/lo/src	

	Add this line to your home directory .bash_profile ( this is the only edit to default .bashrc 
	and .bash_profile
	
		PATH=/opt/lo/bin:$PATH
		
	After an initial startup of cygwin64...a .bash_profile will be automatically created for you, 
	in your home directory.

=========
STEP B - get source files
=========

/Do a git from gerrit.libreoffice.org
	ex:  git clone git://gerrit.libreoffice.org/core

	untar via cygwin to cygwin/home/lo and rename 'libreoffice' directory to 'master'


=========
STEP C - become familiar with two files
=========

/Special_config_filesToReview.
	autogen.input
	config_host.mk

	VERY IMPORTANT==> Replace a downloaded cygwin64/home/lo/master/autogen.input with this github's 
	autogen.input.
	
	Also copy over config_host.mk from this github's autogen.input
	it will have
	export ANT=C:/cygwin64/opt/apache-ant-1.9.2/bin/ant
	export ANT_HOME=C:/cygwin64/opt/apache-ant-1.9.2
	export ANT_LIB=C:/cygwin64/opt/apache-ant-1.9.2/lib
	
	When you run autogen.sh on Step E-3x2... autogen.sh runs configure.ac and many 
	of the entries in config_host.mk should be updated by your env.  But I find that config_host.mk
	isn't thoroughly updated.
	
	
	Do this replacement via cygwin64.  If you copy, paste via Microsoft Explore, cygwin64 might 
	find that file corrupted.... 	You will then need to find a dos2unix.exe utility....  
	I added that utility to this github... if you use it, you 
	will have to copy to c:\cygwin64\bin  and just to be thorough at cygwin64 prompt >chmod +x dos2unix

=========
STEP D - Install Microsoft build environment

=========
/MS.Installs  Review file in that directory.

	Install MS packages.  There is a file in MS.Installs with iso names.

	For all of these packages, it's recommended you first download their iso files.
	Just in case, you need to reinstall.  Also downloading iso file then installing is
	faster then installing these packages onto your machine's directories, via web.

	Install NET 3.0,4.0, 4.2 ( not 4.5 is only needed to do builds with VS2013)
		First complete this build, with  2010, then try 2012.  LibreOffice is still working
		on releasing a 2013 build.
		As far as I can tell, builds are good only for Windows 7 Pro. 
		
	To be on a safe side, after each ISO install,  restart Windows 7 Pro.
	and check for any updates.
		via Windows 7 Desktop. Click on Start/Control Panel then type into <search>:  Update
		Then choose 'Check for Updates'
		Skip this Update step, when you install Visual Studios.
	

	Install MSBuild v 12
	Install Framework SDKs 7.1 8.0 8.1 ( note 8.2 is for VS2013)
	Install VS 2012 Pro ( Trial version.  Get Is
	Install VS 2012 Update 4
	Install VS 2010 Pro (Trial version)
	Install VS 2010 SP 1.

	For VS2010, Install DirectXSDK for a Visual Studio 2010 build.
		You will need to add DirectX to the 
		config_host.mk path.  Edit the PATH by adding path to DirectX SDK

		export PATH=.:/cygdrive/c/Program Files (x86)/Microsoft DirectX SDK (February 2010):

		After I installed DirecXSDK, an inital build required this. ( Maybe I didn't restart my machine>)
	

	install Java JDK 1.8 or a 1.7 32 bit.
	download ant   put into /opt/ant
		ant version is apache-ant-1.9.2
	download junit put into /opt/junit/share/java
		junit version is junit-4.4.jar and junit-4.1.1.jar

	After these microsoft packages are installed, check for any updates.
	Control Panel\System and Security\Windows Update
	Install all.

=========
STEP E - Start build..follow steps below...

=========

		
Step 1:
	cygwin64 should have been installed.
	at prompt check
	/opt/lo/bin/make  

Step 2:  review your /home/.bash_profile


Step 3: start up your cygwin as windows  administrator
	

Step 3X1 Edit cygwin64/home/lo/master/autogen.input  ( edit from cygwin64 using vi or vim )
	VS2010 or VS2012?
		in autogen.input set
		--with-visual-studio=2010  OR
		--with-visual-studio=2012
	Otherwise, for a start..I would recommend using autogen.input as is.
		--with-visual-studio=2012

Step 3x2  RUN AUTOGEN.SH

	cd /opt/lo/master

	you might need to do
	>chmod +x *.sh

	>./autogen.sh

Step 3X3  If autogen has complaints

	If autogen.sh or make complains that a cygwin lib or whatever is missing.
	run start.setup.bat
	Click on cygwin gui "keep'  That says none of your existing cygwin will be downloaded again
	Then do a search in cygwin gui for items that are missing and download without installation.
	Then repeat this process, but install from local directory. (again choose keep, and search for item 
	that needs to be installed)

Step 3X4.  Audit your /home/lo/master/config_host.mk

	At this stage, all of the directories listed in config_host.mk should be local to your machine.
	If not, then perhaps config_host.mk did not update properly.
	Review especially this entry:
	export PATH=  
	you can always add additional paths to that, via vi or vim.  
	
	If you see a path in config_host.mk such as 
	export DOCDIR=/usr/local/share/doc/libreoffice
	
	I just went ahead and created it.  I did not research it.
	Though at end of build... I did not find any documents in that subdirectory.
	
	
	
Step 4 Once autogen.sh is completed Ok. And config_host.mk has been audited, then run make
	

	>/opt/lo/bin/make
		Make will take about four hours.		
	

	if process complains that it lacks a source.ver 
	Add file under Special_config_filesToReview
	Edit using cygwin vi or vim:  If you are building from core, without a version, 
	then edit sources.ver 
	
	sources_ver=master	




Step 5:  After a 1st run of 'make'. Successful or with failure.

	Check that all 3rd Party tars were downloaded
	(at last count there were 90)
	Review autogen.input file

	Edit your autogen.input file by adding:

	--disable-external-fetch

	This will prevent further downloads of 3rd Party tars.

	Then rerun autogen.sh

	>./autogen.sh

	and start again

	>/opt/lo/bin/make 

	Important Note: ====> If 'make' cores.  Ignore.  Just re-start it.
	No need to do >make clean
		
	Right now, we don't have source code for 'make' to debug.  

Step 6:  How to capture make log file and view contents dynamically.

	>/opt/lo/bin/make > m1.log 2>$1

	Then start up a 2nd cygwin64 instance that will give a view into m1.log file with line numbers.
	cd /home/lo/master
	> tail -f m1.log | perl -pe '$_ = "$. $_"'

Step 7:  Generating project files for a Visual Studio 2010 or 2012

	At this stage, you have made an initial decision via autogen.input
		in autogen.input you set
		--with-visual-studio=2010  OR
		--with-visual-studio=2012	

	With either choice, after you have a good build, via cygwin64>/opt/lo/bin/make 
	Then run
	cygwin64>/opt/lo/bin/make vs2012-ide-integration

	This will generate Visual Studio project files, friendly to Visual Studio 2012.

	Once done, in /master you will see a LibreOffice.sln

	If in autogen.input you chose VS2012, then you can go ahead and start up Visual Studio 2012 
	and open up that sln file.

	If you have chosen VS2010, there are two additional steps.

		- open cygwin64/home/lo/master/bin/gbuild-to-ide 
		  and set
			platform_toolset_node.text = 'v100'
			V100 is VS 2010
			v110 is vs 2012

		- run again /opt/lo/bin/make vs2012-ide-integration
		- open LibreOffice.sln and change 2012 to 2010.
			Microsoft Visual Studio Solution File, Format Version 12.00  
			Become for Visual Studio 2010
			Microsoft Visual Studio Solution File, Format Version 11.00

		Then go ahead and start up Visual Studio 2010 and open that solution.
		At this stage, Visual Studio does not have to build anything.  It's all built
		Test it, by building just one of the 338 projects.  
		I would recommend building 	'Executable scalc'

		
Step 8:  See Calc run...  If 'make' build has been completed.

		 Via Windows Explorer
		 go to C:\cygwin64\home\lo\master\instdir\program
		 Double click on scalc.exe
		 
Step 9:	 Debugging In Visual Studio 2012 or 2010
		 + Important.  If 'make' build was ok, there is no need to do a re-build in Visual Studio.
		   And I have not timed how long it would take to re-build.  
		   Though build with 'make' runs about 4 hours ( with 3rd Party tars already downloaded )
		   on a four core 3.7GHZ, with O/S Windows 7 Pro.
		 
		 In Solution Explorer, find 'Executable scalc'
		 
		 + Mark its project as 'Set as startup project'		 
		 + Then in Visual Studio, select Debug/Start Debugging		 
		 + Ignore msg that other projects are out of date.
		 + LibreOffice starts up.
		 + In LibreOffice, choose 'Calc Spreadsheet'
		 + In Calc, click on File, Open
		 + Then go to Top Menu of Visual Studio, and choose
				Debug/Break All
			
		 You can start choosing your break points.
		 
		 This is state of affairs, as I know it.
		 
		 
		 
		 
		




	

	

	





		