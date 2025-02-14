# Running the nicFW Programmer on Linux with WINE

## Requirements
To use the nicFW Programmer on Linux, you will need:
- **Programmer Version**: 2.07.00 or later.
- **Operating System**: This guide is based on **Ubuntu 24.04 LTS ('Noble Numbat')**. If you're using a different Linux distribution, some steps may require adjustments.
- **WINE Version**: At least **WINE 10.0** (latest available version at the time of writing).

## Initial Setup
### 1. Open a Terminal
Launch a Bash terminal to execute the following commands.

### 2. Add Yourself to the `dialout` Group
To allow access to serial ports, check if you are already in the `dialout` group:
```bash
groups
```
If you see `dialout` in the output (e.g., `username adm dialout cdrom sudo dip plugdev users lpadmin`), proceed to 'Installing .NET Runtime'.

If `dialout` is missing, add yourself to the group:
```bash
sudo usermod -aG dialout $USER
newgrp dialout
```

## Installing WINE
### 3. Remove Any Existing WINE Installation
Check your current WINE version:
```bash
wine --version
```
If it reports `wine-10.0` or later, skip to **Step 4: Install .NET Runtime**.

Otherwise, remove any old versions of WINE:
```bash
sudo apt remove wine64 wine32 wine winetricks
rm -rf ~/.wine
```
_Note: Some commands may not be necessary depending on your current setup._

### 4. Install the Latest WINE Version
Add the WINEHQ repository and install WINE:
```bash
sudo mkdir -pm755 /etc/apt/keyrings
wget -O - https://dl.winehq.org/wine-builds/winehq.key | sudo gpg --dearmor -o /etc/apt/keyrings/winehq-archive.key -
sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/noble/winehq-noble.sources
sudo apt update && sudo apt install --install-recommends winehq-stable
```
_Note: If using a different distro, adjust the third command accordingly._

Run the following command to configure WINE:
```bash
winecfg
```
When prompted, install **Mono** and change the Windows version to **Windows 11**, then click **OK**.

## Installing .NET Runtime
1. Download the latest **.NET Desktop Runtime** from [Microsoft's .NET website](https://dotnet.microsoft.com/en-us/download/dotnet/8.0).
   - Look for `.NET Desktop Runtime`, which is currently **version 8.0.13** (this may have changed).
   - Download the `x86` version.
2. Navigate to the download location and run the installer with WINE:
```bash
cd ~/Downloads
wine windowsdesktop-runtime-8.0.13-win-x86.exe
```
_Follow the installation prompts to complete the setup._

## Installing the nicFW Programmer
1. Create a directory to store the programmer:
```bash
mkdir ~/nicfwprog
cd ~/nicfwprog
```
2. Download the programmer zip file:
```bash
wget https://nicsure.co.uk/h3/programmerv2s.zip
```
_Note: If your distro warns about SSL certificates, follow `wget`’s instructions to bypass authentication._

3. Unzip the file:
```bash
unzip programmerv2s.zip
```
_If `unzip` is not installed, install it with:_
```bash
sudo apt install unzip
```

## Connecting Your Radio and Identifying the Serial Port
1. Connect your radio to your computer using a USB-C cable.
2. Identify the correct serial port:
```bash
ls /dev/ | grep ttyUSB
```
- Typically, the port will appear as `/dev/ttyUSB0`.
- If multiple results appear, unplug the radio, rerun the command, and note which entry disappears. That is your radio's port.

### Find the Matching COM Port
1. Launch the programmer once and immediately close it:
```bash
wine nicFWProgrammer.exe
```
2. Find the associated COM port:
```bash
ls ~/.wine/dosdevices -l | grep ttyUSB
```
Example output:
```bash
lrwxrwxrwx 1 username username 12 Feb 14 03:48 com33 -> /dev/ttyUSB0
```
- Note the number after `com` (e.g., `com33`). This is the COM port to select in the programmer.

## Running the Programmer
Start the nicFW Programmer:
```bash
wine nicFWProgrammer.exe
```
- In the **Serial Port** dropdown, select the COM port you identified earlier.
- You are now ready to use the programmer.

## Conclusion
Following these steps, you should have the nicFW Programmer running smoothly on Linux via WINE. Happy programming!
