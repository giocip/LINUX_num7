# num7 STATIC AND DYNAMIC LIBRARIES

It may need: 
        
	sudo mkdir -p /usr/local/lib     #libnum7.a, libnum7.so.1.0.0 file position on system 
	sudo mkdir -p /usr/local/include #num7.h header file position on system 
	sudo mkdir -p /usr/local/bin     #user app/executable file position on system 
	export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH #runtime environment variable by dynamic libraries in .bashrc file for permanent by user  

To get STATIC LIBRARY libnum7.a: 
  
	g++ -std=c++14 -c num7.cpp #num.o STATIC LIBRARY (num7.h in the same compiling folder)  
	ar cr libnum7.a num7.o     #create archive static library libnum7.a  

To get DYNAMIC LIBRARY libnum7.so.1.0.0: 
  
	g++ -std=c++14 -fpic -c num7.cpp 
	g++ -std=c++14 -fpic -o libnum7.so.1.0.0 num7.o -shared -Wl,-soname,libnum7.so.1 

Create the following soft links to libnum7.so.1.0.0 library file: 
	
	ln -s libnum7.so.1.0.0 libnum7.so.1 #create a soname (share object name) symbolic link 
	ln -s libnum7.so.1.0.0 libnum7.so   #create a symbolic link for the library num7 name linker 

and copy all 3 files in the dynamic library directory /usr/local/lib (or /usr/lib/x86_64-linux-gnu) 

To compile app: 

	g++ -std=c++14 test_num7_eligibility.cpp /usr/local/lib/libnum7.a #STATIC LIBRARY a.out (executable file) 
	g++ -std=c++14 test_num7_eligibility.cpp -lnum7 -o b.out          #DYNAMIC LIBRARY b.out (executable file) 

To search library directories on System: 

	ld --verbose | grep SEARCH_DIR 
	g++ -print-search-dirs 

  To check app/executable file: 

	readelf -h /path/to/executable       
