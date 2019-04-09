# cpputest on Windows 10 with Windows Subsystem for Linux (WSL)

## System Setup and WSL configuration
1. Install Windows Subsystem for Linux and Ubuntu
   1. Open Windows Control Panel (Windows Key and type 'Control Panel')
   1. Click on 'Programs'
   1. Click on 'Turn Windows features on or off'
   1. Scroll down and enable 'Windows Subsystem for Linux'
1. Goto the Windows Store and search 'Ubuntu 18.04 LTS'
1. Open Windows Subsystem for Linux (tested with Ubuntu 18.04)
1. Change directories to where you'd like to store the cpputest repo.  Ideally keep this at the same level as the code to be tested.
1. Clone cppUtest (git clone https://github.com/cpputest/cpputest.git)
1. Update all packages 'sudo apt-get update'
1. Install gcc toolchain, autoconf and build tools `sudo apt-get install build-essential autoconf automake libtool g++`


## Make Cpputest (from within WSL)
You can make cpputest in many ways, here are two potential methods from within WSL (I've had the most success with Cmake).

### Using Cmake
1. Change directory to cpputest_build
1. Make with cmake `cmake ..`
1. Run make with `make`
1. Install with `sudo make install`

### Using make
1. Change to the cpputest director `cd /mnt/"directory to cppUtest"/cpputest/`
1. Run autoreconf script './autoreconf.sh"
1. Change directory to cpputest_build
1. Configure the build  `../configure`
1. `make check`
1. `make tdd`
1. `sudo make install`

## Configure path variables after making (to run Cpputest through scripts):
1. From within WSL, add CPPUTEST_HOME to profile, example:
   1. Use vim to edit `vim ~/.profile`
   1. Append line `export CPPUTEST_HOME=/mnt/"directory to cppUtest"/cpputest`
   1. Save file (Esc, x)
1. Copy the library folder from make install, to CPPUTEST_HOME: `cp -a /usr/local/lib $CPPUTEST_HOME`
1. From cmd or Powershell, set the WSL default user `ubuntu config --default-user username` (https://docs.microsoft.com/en-us/windows/wsl/user-support)

## Configuring Cpputest makefile
The Cpputest makefile resides in the local directory and is configured based on the project under test.  As an example, use the makefile from James Grenning: https://github.com/jwgrenning/cpputest-starter-project.

1. Add source files and/or directories of test files (SRC_FILES += and/or SRC_DIRS +=).
1. Add test files and/or directories of test files (TEST_SRC_FILES += and/or TEST_SRC_DIRS +=).
1. Add directory of Mocks, stubs, and fakes (MOCS_SCR_DIRS +=)
1. Eable CppUMock: CPPUTEST_USE_EXTENSIONS = Y
1. Add source include directory (INCLUDE_DIRS +=)
1. Adjust the CppuTest objs directory as needed
1. Adjust the compiler flags
1. Add additional libraries as needed (LD_LIBRARIES += -lgcov for coverage report)
1. Enable coverage reporting: CPPUTEST_USE_GCOV = Y
1. Add project specific defines as needed (example: CPPUTEST_CFLAGS += -DSTEM32F429xx)


## Running Cpputest
### Manually
* From WSL: `make clean`, then `make gcov`
* Windows cmd/Powershell: `bash --login -c "make clean"`, then `bash --login -c "make gcov"` ("--login" is needed to take advantage of the CPPUTEST_HOME path variable.)

**_Note_**: `make gcov` is used to make with coverage report, use `make all` if coverage report is not desired.

### Via a batch file
In essence, runs the same commands from the Windows cmd/Powershell manual steps.  See [cpputest_runner.bat](./cpputest_runner.bat)

# Sources
1. Forked from James Grenning: https://github.com/jwgrenning/cpputest-starter-project
    * A cpputest starter project
	* See the readme directory: [cpputest-starter-kit-readme.pdf](readme/cpputest-starter-kit-readme.pdf) or [cpputest-starter-kit-readme.rtf](readme/cpputest-starter-kit-readme.rtf)
1. https://github.com/miguelmoraperea/guide_setup_cpputest_eclipse_win_7
