# Universal XCFramework build example

Below are instructions for building TDLib for iOS, watchOS, tvOS, visionOS, and also macOS.

If you need only a macOS build for the current architecture, take a look at [TDLib build instructions generator](https://tdlib.github.io/td/build.html).

For example of usage take a look at our [Swift example](https://github.com/tdlight-team/tdlight/tree/master/example/swift).

To compile `TDLib` you will need to:
* Install the latest Xcode via `xcode-select --install` or downloading it from [Xcode website](https://developer.apple.com/xcode/).
  It is not enough to install only command line developer tools to build `TDLib` for iOS. Xcode >= 14.0 is required to build TDLib for watchOS.
* Install other build dependencies using [Homebrew](https://brew.sh):
```
brew install gperf cmake coreutils
```
* If you don't want to build `TDLib` for macOS first, you **must** pregenerate required source code files using `CMake` prepare_cross_compiling target:
```
cd <path to TDLib sources>
mkdir native-build
cd native-build
cmake ..
cmake --build . --target prepare_cross_compiling
```
* Build OpenSSL for iOS, watchOS, tvOS, visionOS, and macOS:
```
cd <path to TDLib sources>/example/ios
./build-openssl.sh
```
Here we use scripts from [Python Apple support](https://github.com/beeware/Python-Apple-support), but any other OpenSSL build should work too.
[Python Apple support](https://github.com/beeware/Python-Apple-support) has known problems with spaces in the path to the current directory, so
you need to ensure that there are no spaces in the path.
Built OpenSSL libraries should be stored in the directory `third_party/openssl/<platform>`, because the next script will rely on this location.
* Build TDLib for iOS, watchOS, tvOS, visionOS, and macOS:
```
cd <path to TDLib sources>/example/ios
./build.sh
```
This may take a while, because TDLib will be built about 16 times.
Resulting XCFramework will work on any architecture and even on a simulator.
We use [CMake/iOS.cmake](https://github.com/tdlight-team/tdlight/blob/master/CMake/iOS.cmake) toolchain, other toolchains may work too.

Built libraries and XCFramework will be stored in `tdjson` directory.

Documentation for all available classes and methods can be found at https://core.telegram.org/tdlib/docs.

If you receive an "error: SDK "appletvsimulator" cannot be located", you need to run the command "sudo xcode-select -s /Applications/Xcode.app/Contents/Developer" before running ./build.sh.
