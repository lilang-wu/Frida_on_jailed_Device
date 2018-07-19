# Frida_on_jailed_Device


### 1. Decompress the IPA file and copy inside the application container (at the same level as the binary) the FridaGadget.dylib file:
```
$ cd Payload/Test.app/
$ curl -O https://build.frida.re/frida/ios/lib/FridaGadget.dylib
```

***
### 2. Modify the binary to insert the load command with the insert_dylib or optool tool
   note: It is necessary to specify the strip-codesign option to ensure the re-signing process works fine later on.  
   [optool](https://github.com/alexzielenski/optool), or [insert_dylib](https://github.com/Tyilo/insert_dylib)
```
$ insert_dylib --strip-codesig --inplace  @executable_path/FridaGadget.dylib Payload/Test.app/Test
or
$ optool install -c load -p "@executable_path/FridaGadget.dylib" -t Payload/Test.app/Test
```

***
### 3. resign IPA file

recommend : 

[ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) or  [iReSign](https://github.com/maciekish/iReSign)

***
### 4. launch App and check
[ios-deploy](https://github.com/ios-control/ios-deploy)
```
$ ios-deploy -d --no-wifi --noinstall -b Payload/Test.app/Test
$ ...
$ frida-ps -Uai
PID  Name    Identifier     
---  ------  ---------------
719  Gadget  re.frida.Gadget
```
