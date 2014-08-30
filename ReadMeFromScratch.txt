This set of instructions originated during August 2014, using LibreOffice master.  
That is a dev environment.
I presume these instructions should apply to any LibreOffice version released as of Aug 2014

This just a kick off set of instructions.  It's expected to work.  
Then you can improve as you see fit.

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
	
	Do this replacement via cygwin64.  If you copy, paste via Microsoft Explore, cygwin64 might 
	find that file corrupted.... 	You will then need to find a dos2unix.exe utility....  
	I added that utility to this github... if you use it, you 
	will have to copy to c:\cygwin64\bin  and just to be thorough at cygwin64 prompt >chmod +x dos2unix

=========
STEP D - Install Microsoft build environment

=========
/MS.Installs  Review file in that directory.

	Install MS packages.  There is a file in MS.Installs with iso names.

	For all of these packages, its recommended you first downloaded their iso files.
	Just in case, you need to reinstall.  Also downloading iso file then installing is
	faster then installing these packages onto your machine's directories, via web.

	Install NET 3.0,4.0, 4.2 ( not 4.5 is only needed to do builds with VS2013)
		First complete this build, with  2010, then try 2012.  LibreOffice is still working
		on releasing a 2013 build.
		As far as I can tell, builds are good only for Windows 7 Pro. 

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

Step 4 Once completed Ok.  Then run make
	

	>/opt/lo/bin/make
		Make will take about four hours.

	if process complains that it lacks a source.ver 
	Add file under Special_config_filesToReview
	Edit.  If you are building from core, without a version, then =master	




Step 5:  After a 1st run of 'make'. Successful or with failure.

	Review autogen.input file
	When you run autogen.sh, it will initially downloaded tar files from LibreOffice.

	Edit your autogen.input file by adding:

	--disable-external-fetch

	This will prevent further downloads of these tars.

	Then redo

	>./autogen.sh

	and start again

	>/opt/lo/bin/make 



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




	

	

	





		