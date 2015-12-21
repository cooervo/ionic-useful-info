# Useful ionic commands

Create a local server that updates the app in the **browser** as changes are saved.
  
      ionic serve 
      
Updates saved changed in the app in the **android device** VERY USEFUL!!!
  
      ionic run android -l -c

### Run chrome as a non secure instance for VPN and ajax

use this commmand inside the desktop link (acceso directo) of chrome, **make sure to close any other running process in the background of chrome (cntrl+alt+supr close every chrome process) and close chrome first**

    "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --args --disable-web-security


-------------------------------------------

# Android Builds

A step by step tutorial of how to build ionic apps for android with and without crosswalk.

## Non crosswalk build


1. To generate a release build for Android, first go to ionic app root directory.

2. Then we can use the following cordova terminal command:

        #Generates the *-unsigned.apk file
        cordova build --release android

3. Go to directory `platforms/android/build/outputs/apk`, where we can find our generated unsigned APK file.
**Very important: if the past command generates more than 1 apks, use the one that contains `arm` in its name for example: `android-armv7-release-unsigned.apk`.**

4. Generate private key using the keytool command that comes with the JDK. If this tool isn't found, refer to the installation guide (If this command doesn't work on windows make it on linux or a mac and pass *.keystore file through dropbox):

        #This generates key.keystore file
         keytool -genkey -v -keystore key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000

5. To sign the unsigned APK, run the jarsigner tool which is also included in the JDK. **The jarsigner signs the unsigned.apk file** (If jarsigner isn't recognize as command just add it to environment vars PATH, usually the file is where the java bin are):

         #Remember to rename key.keystore and unsigned.apk by whatever this files are named in your system
         jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore key.keystore unsigned.apk alias_name

6. Finally, we need to run the zip align tool to optimize the APK (If zipalign is not recognize try adding it to environment vars PATH for windows; usually the zipalign tool can be found in `your/path/to/Android/sdk/build-tools/VERSION/zipalign`)
    
        #If the zipalign isn't recognize try ./zipalign for gitbash or unix based systems 
        zipalign -v 4 unsigned.apk YouAppName.apk

## If you app has crosswalk

#### Pre-Build instructions

On Android, the app depends on the Crosswalk browser, and you must have in linked an installed before buiding, after generating the platform files

1. You'll need to add the corresponding ios platform and install plugins

    	# ionic platform add android
	
2. Add the crosswalk plugin and required dependencies

    	# ionic browser add crosswalk

3. Remember to test the app in the simulator on in GenyMotion before building

    	# ionic run android

#### Build instructions

1. Go to the key store directory in the APP root

1. To generate a release build for Android, first go to ionic app root directory .

2. Then we can use the following cordova terminal command:

        #Generates the *-unsigned.apk file
        cordova build --release android

3. Go to directory `platforms/android/build/outputs/apk`, where we can find our generated unsigned APK file.
**Very important: if the past command generates more than 1 apks, use the one that contains `arm` in its name for example: `android-armv7-release-unsigned.apk`, unless you are building for x86 devices.**

4. A valid key for signing the APP is already generated and available in toe android keystore directory in the project root.

5. To sign the unsigned APK, run the jarsigner tool which is also included in the JDK. **The jarsigner signs the unsigned.apk file** (If jarsigner isn't recognize as command just add it to environment vars PATH, usually the file is where the java bin are):

         #CD to the keystore directory
         cd android-keystore
         
         # Sign the generated apk
         #Remember to rename unsigned.apk by whatever this files are named in your system
         jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore key.keystore ../APP/platforms/android/build/outputs/apk/unsigned.apk azanglogold

6. Finally, we need to run the zip align tool to optimize the APK (If zipalign is not recognize try adding it to environment vars PATH for windows; usually the zipalign tool can be found in `your/path/to/Android/sdk/build-tools/VERSION/zipalign`)
    
        # If the zipalign isn't recognize try ./zipalign for gitbash or unix based systems
        # Remember to rename unsigned.apk by whatever this files are named in your system
        zipalign -v 4 ../APP/platforms/android/build/outputs/apk/unsigned.apk AZSeguros.apk


----------------------------------------------------------
