# rust

This repository contains:
Markup :  [esp-rs/rust](https://github.com/esp-rs/rust/ "Named link title") and http://www.google.fr/ or <http://example.com/>

Workflows for building a Rust fork esp-rs/rust with Xtensa support
Binary artifacts in Releases
An installation script to locally install a pre-compiled nightly ESP32 toolchain
If you want to know more about the Rust ecosystem on ESP targets, see The Rust on ESP Book chapter

Table of Contents
rust-build
Table of Contents
Xtensa Installation
espup installation
Download installer in Bash
Linux and macOS
Prerequisites
Installation commands
Set up the environment variables
Arguments
Windows Long path limitation
Windows x86_64 MSVC
Prerequisites x86_64 MSVC
Windows x86_64 GNU
Prerequisites x86_64 GNU
Long path limitation
RISC-V Installation
Building projects
Cargo first approach
Idf first approach
Using Containers
Using Dev Containers
Xtensa Installation
Deployment is done using espup

espup installation
cargo install espup
espup install # To install Espressif Rust ecosystem
# [Unix]: Source the following file in every terminal before building a project
. $HOME/export-esp.sh
Or, downloading the pre-compiled release binaries:

Linux aarch64
curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-aarch64-unknown-linux-gnu -o espup
chmod a+x espup
./espup install
# Source the following file in every terminal before building a project
. $HOME/export-esp.sh
Linux x86_64
curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-unknown-linux-gnu -o espup
chmod a+x espup
./espup install
# Source the following file in every terminal before building a project
. $HOME/export-esp.sh
macOS aarch64
curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-aarch64-apple-darwin -o espup
chmod a+x espup
./espup install
# Source the following file in every terminal before building a project
. $HOME/export-esp.sh
macOS x86_64
curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-apple-darwin -o espup
chmod a+x espup
./espup install
# Source the following file in every terminal before building a project
. $HOME/export-esp.sh
Windows MSVC
Invoke-WebRequest 'https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-pc-windows-msvc.exe' -OutFile .\espup.exe
.\espup.exe install
Windows GNU
Invoke-WebRequest 'https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-pc-windows-msvc.exe' -OutFile .\espup.exe
.\espup.exe install
For Windows MSVC/GNU, Rust environment can also be installed with Universal Online idf-installer: https://dl.espressif.com/dl/esp-idf/

Download installer in Bash
Deprecated method

curl -LO https://github.com/esp-rs/rust-build/releases/download/v1.73.0.0/install-rust-toolchain.sh
chmod a+x install-rust-toolchain.sh
Linux and macOS
The following instructions are specific for the ESP32 and ESP32-S series based on Xtensa architecture.

Instructions for ESP-C series based on RISC-V architecture are described in RISC-V section.

Prerequisites
Linux:
Dependencies (command for Ubuntu/Debian):
apt-get install -y git curl gcc clang ninja-build cmake libudev-dev unzip xz-utils \
python3 python3-pip python3-venv libusb-1.0-0 libssl-dev pkg-config libpython2.7
No prerequisites are needed for macOS.

Installation commands
git clone https://github.com/esp-rs/rust-build.git
cd rust-build
./install-rust-toolchain.sh
. ./export-esp.sh
Run ./install-rust-toolchain.sh --help for more information about arguments.

Installation of different version of the toolchain:

./install-rust-toolchain.sh --toolchain-version 1.73.0.0
. ./export-esp.sh
Set up the environment variables
We need to update environment variables as some of the installed tools are not yet added to the PATH environment variable, we also need to add LIBCLANG_PATH environment variable to avoid conflicts with the system Clang. The environment variables that we need to update are shown at the end of the install script and stored in an export file. By default this export file is export-esp.sh but can be modified with the -f|--export-file argument.

We must set the environment variables in every terminal session.

Note If the export variables are added to the shell startup script, the shell may need to be refreshed.

Arguments
-b|--build-target: Comma separated list of targets [esp32,esp32s2,esp32s3,esp32c3,all]. Defaults to: esp32,esp32s2,esp32s3
-c|--cargo-home: Cargo path.
-d|--toolchain-destination: Toolchain installation folder. Defaults to: <rustup_home>/toolchains/esp
-e|--extra-crates: Extra crates to install. Defaults to: ldproxy cargo-espflash
-f|--export-file: Destination of the export file generated. Defaults to: export-esp.sh
-i|--installation-mode: Installation mode: [install, reinstall, uninstall]. Defaults to: install
-k|--minified-llvm: Use minified LLVM. Possible values: [YES, NO]. Defaults to: YES
-l|--llvm-version: LLVM version.
-m|--minified-esp-idf: [Only applies if using -s|--esp-idf-version]. Deletes some idf folders to save space. Possible values [YES, NO]. Defaults to: NO
-n|--nightly-version: Nightly Rust toolchain version. Defaults to: nightly
-r|--rustup-home: Path to .rustup. Defaults to: ~/.rustup
-s|--esp-idf-version: ESP-IDF branch to install. When empty, no esp-idf is installed. Default: ""
-t|--toolchain-version: Xtensa Rust toolchain version
-x|--clear-cache: Removes cached distribution files. Possible values: [YES, NO]. Defaults to: YES
Windows Long path limitation
Several build tools have problem with long paths on Windows including Git and CMake. We recommend to put project on short path or use command subst to map the directory with the project to separate disk letter.

subst "R:" "rust-project"
Windows x86_64 MSVC
The following instructions are specific for the ESP32 and ESP32-S series based on Xtensa architecture. If you do not have Visual Studio and Windows 10 SDK installed, consider the alternative option Windows x86_64 GNU.

Instructions for ESP-C series based on RISC-V architecture are described in RISC-V section.

Prerequisites x86_64 MSVC
Installation of prerequisites using Winget:

winget install --id Git.Git
winget install Python # requirements for ESP-IDF based development, skip in case of Bare metal
winget install -e --id Microsoft.WindowsSDK
winget install Microsoft.VisualStudio.2022.BuildTools --silent --override "--wait --quiet --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64"
Installation of prerequisites using Visual Studio installer GUI - installed with option Desktop development with C++ - components: MSVCv142 - VS2019 C++ x86/64 build tools, Windows 11 SDK

Visual Studio Installer - configuration

Installation of MSVC and Windows 11 SDK using vs_buildtools.exe:

Invoke-WebRequest 'https://aka.ms/vs/17/release/vs_buildtools.exe' -OutFile .\vs_buildtools.exe
.\vs_BuildTools.exe --passive --wait --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 --add Microsoft.VisualStudio.Component.Windows10SDK.20348
Installation of prerequisites using Chocolatey (run PowerShell as Administrator):

Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
choco install visualstudio2022-workload-vctools windows-sdk-10.0 -y
choco install cmake git ninja python3 -y  # requirements for ESP-IDF based development, skip in case of Bare metal
Main installation:

Invoke-WebRequest 'https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-pc-windows-msvc.exe' -OutFile .\espup.exe
.\espup.exe install
Windows x86_64 GNU
The following instructions describe deployment with the GNU toolchain. If you're using Visual Studio with Windows 10 SDK, consider option Windows x86_64 MSVC.

Prerequisites x86_64 GNU
Install MinGW x86_64 e.g., from releases https://github.com/niXman/mingw-builds-binaries/releases and add bin to environment variable PATH

choco install 7zip -y
Invoke-WebRequest https://github.com/niXman/mingw-builds-binaries/releases/download/12.1.0-rt_v10-rev3/x86_64-12.1.0-release-posix-seh-rt_v10-rev3.7z -OutFile x86_64-12.1.0-release-posix-seh-rt_v10-rev3.7z
7z x x86_64-12.1.0-release-posix-seh-rt_v10-rev3.7z
$env:PATH+=";.....\x86_64-12.1.0-release-posix-seh-rt_v10-rev3\mingw64\bin"
Main installation:

Invoke-WebRequest 'https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-pc-windows-msvc.exe' -OutFile .\espup.exe
.\espup.exe install
Long path limitation
Several build tools have problem with long paths on Windows including Git and CMake. We recommend to put project on short path or use command subst to map the directory with the project to separate disk letter.

subst "R:" "rust-project"
RISC-V Installation
The following instructions are specific for ESP32-C based on RISC-V architecture.

Install the RISC-V target for Rust:

rustup target add riscv32imc-unknown-none-elf
Building projects
Cargo first approach
Install cargo-generate

cargo install cargo-generate
Generate project from template with one of the following templates

# STD Project
cargo generate esp-rs/esp-idf-template cargo
# NO-STD (Bare-metal) Project
cargo generate esp-rs/esp-template
To understand the differences between the two ecosystems, see Ecosystem Overview chapter of the book. There is also a Chapter that explains boths template projects:

std template explanation
no_std template explanation
Build and flash:

cargo espflash flash <SERIAL>
Where SERIAL is the serial port connected to the target device.

cargo-espflash also allows opening a serial monitor after flashing with --monitor option.

If no SERIAL argument is used, cargo-espflash will print a list of the connected devices, so the user can choose which one to flash.

See Usage section for more information about arguments.

If espflash is installed (cargo install espflash), cargo run will build, flash the device, and open a serial monitor.

If you are looking for inspiration or more complext projects see:

Awesome ESP Rust - Projects Section
Rust on ESP32 STD demo app
Idf first approach
When building for Xtensa targets, we need to override the esp toolchain, there are several solutions: - Set esp toolchain as default: rustup default esp - Use cargo +esp - Override the project directory: rustup override set esp - Create a file called rust-toolchain.toml or rust-toolchain with: toml [toolchain] channel = "esp" 

Get example source code

git clone https://github.com/espressif/rust-esp32-example.git
cd rust-esp32-example-main
Select architecture for the build

idf.py set-target <TARGET>
Where TARGET can be:

esp32 for the ESP32(Xtensa architecture). [Default]
esp32s2 for the ESP32-S2(Xtensa architecture).
esp32s3 for the ESP32-S3(Xtensa architecture).
Build and flash

idf.py build flash
