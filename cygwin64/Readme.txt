	+ get the setup-x86_64.exe from cygwin.org
	+ run it by using start.setup.bat
		- start.setup.bat will fetch cygwin related packages to build LibreOffice for a win32 platform.
	+ Run it twice.
		1. First download without installing.
			1.1.  Choose a local directory in c:/cygwin64/install
		2. Also if you have bandwidth, choose a cygwin site like
			2.1   http://mirror.mcs.anl.gov  
			2.2   They have bandwidth on their side
		3. After it downloads, then restart this install and choose
			3.1. Install from local directory.

	Then start up cygwin64 and at prompt type in
	>cpan -i Archive::Zip
	This packager is needed and is missing.

	Install ant in c:/cygwin64/opt/apache-ant-1.9.2
	Install junit in c:/cygwin6/opt/junit/share/java/junit-4.4.jar & junit-4.1.1.jar

	Create via Cygwin dir:  /opt/lo/bin 
		copy this make.exe here.  chmod+x on it.  IT is a 64bit version

	Add this line to your home directory .bash_profile ( this is the only edit to default .bashrc and .bash_profile
		PATH=/opt/lo/bin:$PATH
	After an initial startup of cygwin64...a .bash_profile will be automatically created for you, in your home directory.