# MPC
Make Project Creator 
MPC to create shared libraries, and link against them from an executable
Shared libraries are binaries containing symbols that are resolved at runtimes, instead of linked directly into the library at compile-time. 

## shared_lib.mpc
    
    project (my_shared_lib) {
      sharedname = my_shared_lib     // Declare project as shared library
      
      dynamicflags += MY_SHARED_LIB_BUILD_DLL
      
      dllout = .                     // Define location of generated shared library
      libout = .                     // Define location of import library (Windows only)
      
      Source_Files {
        my_shared_lib.cpp
      }
## generate the export header      
If you are building against the ACE library, then use ACE to generate the proper export header file:

     %> generate_export_file.pl MY_SHARED_LIB > my_shared_lib_export.h
     
 
## my_shared_lib.h
    
    #include <string>
      
    #include "my_shared_lib_export.h"      // include export header
    
    MY_SHARED_LIB_Export
    void print_message (const std::string & msg);
    
    class MY_SHARED_LIB_Export Greeting
    {
    public:
      Greeting (void);
      ~Greeting (void);
      
      void say_message (const std::string & msg);
    };
 
## my_shared_lib.cpp
    
    #include "my_shared_lib.h"
    #include <iostream>
          
    void print_message (const std::string & msg)
    {
      std::cout << msg << std::endl;
    }
    
    Greeting::Greeting (void) { }
    Greeting::~Greeting (void) { }
      
    void Greeting::say_message (const std::string & msg)
    {
      std::cerr << msg << std::endl;
    }
    
## Using the Shared Library in an Executable

shared_lib.mpc

    project (shared_exe) {
      exename = shared_exe
      install = .
      
      libs  += my_shared_lib
      after += my_sahred_lib

      Source_Files {
        shared_exe.cpp
      }
    }
    
    
## source code for shared_exe.cpp contains

shared_exe.cpp
    
    #include "my_shared_lib.h"
    
    int main (int argc, char * argv [])
    {
      print_message ("Hello, World!");
      
      Greeting g;
      g.say_message ("Hello, World!");
      
      return 0;
    }
    
now compile the executable project, and it will link against the shared library

https://cs.iupui.edu/~hilljh/faqs/shared-library.php
