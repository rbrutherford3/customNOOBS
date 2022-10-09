# customnoobs
[NOOBS](https://github.com/raspberrypi/noobs) is a package for the [Raspberry Pi](https://www.raspberrypi.org/) system that allows a user to load an operating system on their Pi's SD card without the use of imaging software.  The most common operating sytem is called [Raspbian](https://www.raspberrypi.org/downloads/raspbian/), which is a derivation of Debian.  `customnoobs` allows the user to create their own customized NOOBS setup with Raspbian.  The user chooses their own scripts to be run upon the *first boot* of Raspbian.  This is ideal for installations and configurations that a user would not want to repeat every time they flashed their Raspberry Pi's SD card.  Headless installations were in mind when this program was written.  Written to be used in conjunction with another project of mine, [initpi](https://github.com/rbrutherford3/initpi) for automating secure headless setups.

###  Short Instructions:
1. Create/select some custom script(s) to run on the first boot
1. Download `customnoobs` files anywhere on your linux system
1. Run `customnoobs`, passing in your script location(s) as arguments
1. Wait up to an hour (compressing `root.tar.xz` is a long process)
1. Copy contents of newly created `NOOBS` directory onto a freshly formatted SD card
1. Insert into your Raspberry Pi, power on, and wait!  If all went well, your custom scripts should have run after the first boot.

It is advisable to test your scripts before using this program to compile a custom NOOBS package

### Detailed Instructions:
1. Download `customnoobs` files anywhere on your system
1. Run `customnoobs` as `root` and provide the following arguments (in no particular order):
    1. Absolute or relative path of custom script
    1. Absolute or relative path of custom script directory (optional: see *Multiple Files*)
    1. Raspbian version to download (optional: `lite` or `full` only, default is normal distribution)
1.  Wait up to an hour for custom `NOOBS` directory to be created in current working directory (CWD).  For this reason, you may wish to run the process in the background.
1.  Format an SD card
1.  Copy contents of `NOOBS` folder (not `NOOBS` folder itself) onto the SD card
1.  Load SD card into Raspberry Pi
1.  Power on Raspberry Pi
1.  Wait for Raspbian to complete installing and custom scripts to finish running
  
### Multiple Files
In some cases you may need multiple files for your customized installation, whether it's multiple scripts, or a configuration file, etc.  If that is the case, you may provide a directory location along with a file location.  All files from the directory will be *recursively* copied into the customized Raspbian.  You must still provide a file location to indicate which script needs to run first.  Keep in mind that unless the script you provide references other scripts in the directory, they will not run.  The file location must also be an absolute path or a relative path to the CWD.  Providing just the filename (i.e.: `script.sh`) will not work.  The file must also be in the 1st level of the directory provided (i.e.: `/../../[dir]/[file]`).  If you are concerned about relative references to other files failing, you may use the following absolute path: `/usr/local/bin/firstboot/[dir]/` where `[dir]` is the name of the parent directory provided.

### Ownership and permissions
All files will be copied as `root`, so the owner of all directories and files copied will become `root`.  Permissions will be unaltered, so make sure that the script you provided is executable.

### How it works
The most recent copy of NOOBS and Raspbian are downloaded and `root.tar.xz` is extracted. The user file(s) are copied to `/usr/local/bin/firstboot/` (or `/usr/local/bin/firstboot/[dir]`, if multiple files).  `rc.local` is backed up and altered to run a file `/usr/local/bin/firstboot` that waits until the first startup is compete, restores `rc.local` to it's original state, and calls the user script.  Some minor alterations are made, including setting up a silent (non-interactive) installation of Raspbian.  `root.tar.xz` is repacked, which take the longest amount of time.

### Acknowledgments
Thank you very much to Harry Fairhead and his article ["Real Raspberry Pi - Getting Started And Custom NOOBS"](https://kbroman.org/github_tutorial/pages/init.html) for being the chief source of information in writing this project
