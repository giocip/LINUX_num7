# STATIC AND DYNAMIC num7 LIBRARIES
## _DYNAMIC LIBRARY INSTALLATION:_
Create the following soft links to libnum7.so.1.0.0 file :
	
	ln -s libnum7.so.1.0.0 libnum7.so.1 #create a soname symbolic link
	ln -s libnum7.so.1.0.0 libnum7.so  #create a symbolic link for the library num7 name linker

and copy all 3 files in the dynamic library directory /lib/x86_64-linux-gnu
