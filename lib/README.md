# num7 STATIC AND DYNAMIC LIBRARIES

It may need: 
        
	sudo mkdir -p /usr/local/lib     #libnum7.a, libnum7.so.1.0.0 file position on system 
	sudo mkdir -p /usr/local/include #num7.h header file position on system 
	sudo mkdir -p /usr/local/bin     #user app/executable file position on system 
	export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH #runtime environment variable by dynamic libraries in .bashrc file for permanent by user  

To get STATIC LIBRARY libnum7.a: 
  
	g++ -std=c++14 -fpic -c num7.cpp #num.o STATIC LIBRARY (num7.h in the same compiling folder)  
	ar cr libnum7.a num7.o     #create archive static library libnum7.a  

To get DYNAMIC LIBRARY libnum7.so.1.0.0: 
  
	g++ -std=c++14 -fpic -c num7.cpp 
	g++ -std=c++14 -fpic -o libnum7.so.1.0.0 num7.o -shared -Wl,-soname,libnum7.so.1 

Create the following soft links to libnum7.so.1.0.0 library file: 
	
	ln -s libnum7.so.1.0.0 libnum7.so.1 #create a soname (share object name) symbolic link 
	ln -s libnum7.so.1.0.0 libnum7.so   #create a symbolic link for the library num7 name linker 

and copy all 3 files in the dynamic library directory /usr/local/lib (or /usr/lib/x86_64-linux-gnu) 

To compile app: 

	g++ -std=c++14 test_num7_eligibility.cpp /usr/local/lib/libnum7.a #DEBIAN => STATIC LIBRARY a.out (executable file)
 	g++ -std=c++14 test_num7_eligibility.cpp /lib64/libnum7.a 	  #FEDORA => STATIC LIBRARY a.out (executable file)
	g++ -std=c++14 test_num7_eligibility.cpp -lnum7 -o b.out          #DYNAMIC LIBRARY b.out (executable file) 

To search library directories on System: 

	ld --verbose | grep SEARCH_DIR 
	g++ -print-search-dirs 

To check app/executable file: 

	readelf -h /path/to/executable       

To BUILD-debug_num7_library_and_DEBIAN_PACKAGE file (debug):
	
	echo 'BULDING num7 library and debian package, PLEASE BE PATIENT...'
	cd LINUX_num7
	g++ -O0 -g -Wall -Wextra -fsanitize=address,undefined -fpic -c num7.cpp
	g++ -O0 -g -Wall -Wextra -fsanitize=address,undefined -fpic -o libnum7.so.1.0.0 num7.o -shared -Wl,-soname,libnum7.so.1
	ln -s libnum7.so.1.0.0 libnum7.so.1 #create a soname symbolic link
	ln -s libnum7.so.1.0.0 libnum7.so #create a symbolic link for the library name linker
	g++ -O0 -g -Wall -Wextra -fsanitize=address,undefined -fpic -c num7.cpp # => num.o STATIC LIBRARY
	ar cr libnum7.a num7.o #create archive static library libnum7.a
	mkdir num7 && mkdir num7/DEBIAN
	chmod 755 num7
	chmod 755 num7/DEBIAN
	mkdir -p num7/lib/x86_64-linux-gnu #GENERAL LINUX SYSTEM-LIBRARY DIRECTORY
	mkdir -p num7/usr/include
	chmod 755 num7/usr && chmod 755 num7/usr/include
	cp -P libnum7* num7/lib/x86_64-linux-gnu
	#cp -P libnum7.a num7/lib/x86_64-linux-gnu
	cp -P num7.h num7/usr/include
	cp ../control ./num7/DEBIAN #copy control file with metadata library in current directory
	dpkg-deb --build num7
	cp num7.deb .. #previous directory
	cp -P libnum7* ..
	if [ $? -eq 0 ]
	then
	  echo -e "\n*** SUCCESS ***"
	  exit 0
	else
	  echo -e "\n### FAILURE ###" >&2
	  exit 1
	fi

To BUILD_num7_dyn_library_and_DEBIAN_PACKAGE (production):

	rm -Rf LINUX_num7
	git clone https://github.com/giocip/LINUX_num7
	echo 'BULDING num7 library and debian package, PLEASE BE PATIENT...'
	cd LINUX_num7
	g++ -fpic -c num7.cpp
	g++ -fpic -o libnum7.so.1.0.0 num7.o -shared -Wl,-soname,libnum7.so.1
	ln -s libnum7.so.1.0.0 libnum7.so.1 #create a soname symbolic link
	ln -s libnum7.so.1.0.0 libnum7.so #create a symbolic link for the library name linker
	g++ -fpic -c num7.cpp # => num.o STATIC LIBRARY
	ar cr libnum7.a num7.o #create archive static library libnum7.a
	mkdir num7 && mkdir num7/DEBIAN
	chmod 755 num7
	chmod 755 num7/DEBIAN
	mkdir -p num7/lib/x86_64-linux-gnu #GENERAL LINUX SYSTEM-LIBRARY DIRECTORY
	mkdir -p num7/usr/include
	chmod 755 num7/usr && chmod 755 num7/usr/include
	cp -P libnum7* num7/lib/x86_64-linux-gnu
	#cp -P libnum7.a num7/lib/x86_64-linux-gnu
	cp -P num7.h num7/usr/include
	cp ../control ./num7/DEBIAN #copy control file with metadata library in current directory
	dpkg-deb --build num7
	cp num7.deb .. #previous directory
	cp -P libnum7* ..
	if [ $? -eq 0 ]
	then
	  echo -e "\n*** SUCCESS ***"
	  exit 0
	else
	  echo -e "\n### FAILURE ###" >&2
	  exit 1
	fi

To control file:
	
	Package: num7
	Version: 1.0.0
	Maintainer: https://github.com/giocip/LINUX_num7
	Architecture: amd64
	Description: C++ ARBITRARY PRECISION ARITHMETIC-LOGIC DECIMAL LIBRARY FOR LINUX KERNEL 6: ls -l /lib/x86_64-linux-gnu/libnum7* /usr/include/num7.h => 5 files

To clean.sh file:

 	#!/bin/bash
	rm -Rf LINUX_num7
	rm -Rf libnum7*
	rm -Rf num7.deb

To RED-FAT/FEDORA /root/rpmbuild/SPECS/num7.spec file:
	
	Name:           num7
	Version:        1.0.0
	Release:        8
	Summary:        C++ ARBITRARY PRECISION ARITHMETIC-LOGIC DECIMAL LIBRARY FOR LINUX KERNEL 6 BY AMD64 SYSTEMS
	
	License:        MIT
	URL:            https://github.com/giocip/LINUX_num7
	Source0:        https://github.com/giocip/LINUX_num7/lib
	Vendor:         giocip
	
	%description
	num7 library dir: ls -l /lib64/libnum7* /usr/include/num7.h => 5 files
	
	%prep
	
	%build
	
	%install
	mkdir -p %{buildroot}/usr/lib64
	mkdir -p %{buildroot}/usr/include
	cp /root/rpmbuild/SOURCES/libnum7.so.1.0.0 %{buildroot}/usr/lib64/
	cp /root/rpmbuild/SOURCES/libnum7.a        %{buildroot}/usr/lib64/
	cp /root/rpmbuild/SOURCES/num7.h           %{buildroot}/usr/include/
	ln -sf libnum7.so.1.0.0 %{buildroot}/usr/lib64/libnum7.so.1 #create a soname symbolic link
	ln -sf libnum7.so.1.0.0 %{buildroot}/usr/lib64/libnum7.so   #create a soname symbolic link
	
	%files
	/usr/lib64/libnum7.a
	/usr/lib64/libnum7.so.1.0.0
	/usr/lib64/libnum7.so.1
	/usr/lib64/libnum7.so
	/usr/include/num7.h
	
	%changelog
	* Sun May 11 2025 Giovanni Cipriani <giocip7@gmail.com> - 1.0.0-8
	- C++ ARBITRARY PRECISION ARITHMETIC-LOGIC DECIMAL LIBRARY FOR LINUX KERNEL 6 BY AMD64 SYSTEMS

To build_rpm_num7_package.sh:

	#!/bin/bash
	rpmbuild -ba num7.spec
