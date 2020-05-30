# CoreDOS
FreeDOS Distro based around a simple to use cli enviroment.

This is nothing more than a braindump of ideas I have for this project. Next to no work has been done on it other than a very basic proof of concept.

## Main goals
- I want CoreDOS to act as a standard syntax layer for all common DOS activities. To allow for these commands to be as hardware agnostic as possible. It will attempt to autodetect and initialize your hardware while using as little resources as possible. Think of CoreDOS like the Gnu part of Gnu/Linux...
- When autodetection is not possible, there will be a simple set of config files that can tell the hardware what to do so it'll still initialize properly, and use the same commands and syntax as everything else.
- This will allow for super easy to use frontends for DOS games that'll configure all your hardware/memory settings for you. (I've done this already in a proof on concept.) LaunchBox for DOS is my current favorite frontend.
- I will be including lots of 3rd party tools and drivers to make CoreDOS a drop-in solution for 99% of machines. Meaning ZERO configuration required for most hardware. Games however will require a little bit extra configuration.

## Secondary goals
- First secondary goal is for CoreDOS to be as close to 100% compatible with DosBOX's syntax as possible. Also I would like to have a program that'll interpret the dosbox .conf file and act apropriately. (This is probably not possible)
- Second secondary goal is to be easilly used with archival projects like GoG and eXoDOS. Just drop the folder in, and the game will play. (This'll likely require the goal above)
- Third secondry goal Integrate a Win32>DOS wrapper and script DosBOX for win9x to launch under native DOS. The scripts will, if not given a .conf file, automap all detected drives to DosBOX making it seem as if it's running on real hardware. This should enable certain picky games to run. (speed sensitive, or requiring odd soundcards, MT-32, etc...) This will also be the useful for systems without proper sb16 support that are still compatible with the wrapper. (some ac97 soundchips)
- Fourth secondary goal is to create a couple of trimmed down instances of Linux, which will then run DosBOX in a simular manner as the option above. This can be launched from the boot menu, and maybe even as it's own command. It'll probably just reboot when DosBOX is closed. This distro will autoconfigure attached drives, sound and video drivers, as well as full network support. (maybe even a ftp server?)
- Fifth Goal, add EFI support for newer machines. This will use the Linux booting method listed above.
- Sixth Goal, add other DOS emulation platforms. PCem, DOSemu, QEMU, etc...
- Seventh Goal, add support for updating it's self over the network. (add a package manager)
- Eigth Goal, make a CoreDOS frontend with a Mousedriven text user interface and easy to enable features for each game/program. (enable sound, networking, mouse, cd-rom, etc...)
- Ninth Goal, make a Raspberry Pi version?

## How I plan on accomplishing this...
- use freedos as a base.
- use as many unix/linux commands as possible
- config.sys will be as bare as possible to allow for dynamic loading/unloading of drivers.

linux like folder structure...
- c:\cdos\bin\ < -- core OS program stuff, and .bat files pointing to 3rd party programs, this is what'll be in %path% by default.
- c:\cdos\etc\ < -- config files and other scripts
- c:\cdos\drv\ < -- various 3rd party drivers
- c:\cdos\opt\ < -- various 3rd party tools

etc structure...
- \etc\hosts hosts file
- \etc\init\ for startup scripts. Might need to modify freedos kernel for this (not sure about this one, might just have a complex autoexec system instead). When these scrips are done, then launch rc.local and then autoexec.bat
- \etc\sound\ for sound scripts, it'll attempt to autodetect your card and use the appropriate drivers. Should work with 90% of vintage sound cards. Has templates for other 10%. Most scripts can be unloaded when done.
- \etc\network\ for net scripts, this is off by default. Might include other command to auto configure this section.
- \etc\cdrom\ for CD-ROM scripts, it'll attempt to use a set of safe defaults that work with 99% of drives. Will include templates for other common setups. Most scripts can be unloaded when done.
- \etc\mouse\ for Mouse scripts, it'll attempt to use a set of safe defaults that work with 99% of Mice. Will include templates for other common setups. Most scripts can be unloaded when done.
- \etc\memory\ for Memory scripts, it'll include several different templates to switch between depending on the game/program. It's off by default, and most scripts can be unloaded when done. This is half of cdos' killer feature.
- \etc\mount\ for complex Mounting scripts. It'll include several scripts to handle iso, img, floppy and network path mounting. These can also be unloaded when done. This the other half cdos' killer feature.
- \etc\video\ for Video scripts, off by default, but will include a few templates for different gfx cards to increase performance.
- \etc\slow\ for various cpu speed scripts. This will allow for slowdown templates to be called for different games.

Programs to Make
- make a symlink driver/tsr that'll fake symlink compatibility. Usefull for certain possible unix directory layouts. (minor tweak to existing program)
- make a mount command with same syntax as dosbox allow for mounting virtual floppies, cdroms, hdds, etc to both directories and to img files. (basically done already)
- make a TSR that obeys the speed hotkeys of dosbox
- make a dosbox.conf interpreter (probably not possible) It'll read the soundcard and speed settings and act appropriately. It'll then execute the autoexec section...
- chroot but with dos?? (probably not possible)
- integrate actual DOSBOX using a win32-DOS wrapper.

If sidegoals are ever completed, categorize it into 4 compatibility modes.
- 8088 -> RealMode only, meant for OLD computers (custom FreeDOS Kernel, drivers, utilities, etc...)
- 1990-2000 -> Native DOS mode, with option for DOSBOX emulation.
- 2000-2010 -> Old Linux Wrapper, boots to a old version of linux. Linux then starts a fullscreen DOSBox or DOSemu instance and automaps all detected drives. Mounts root as Z (just like Wine)
- 2010-present -> New Linux Wrapper, boots to a current version of Linux. Linux then starts a Fullscreen instance of either DOSBox, DOSemu or PCEm. It will automount all detected drives and mount the root as Z when possible.
- EFI-Boot -> If EFI is used to boot than it'll just use the 2010-Present method.

DOSBox, DOSEmu, PCem will run a CDOS from a floppy image, which will then call all the non-hardware specific config and bin files from the c drive. It should boot the same in this mode as it should in Native Mode.
