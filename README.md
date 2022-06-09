# AndroidAppBundle
Repository contains the Android App Bundle creation guideline, benefits and discuss regarding the dynamic features.

AAB file extension is a new publishing format of Android App package over the APK. In AAB package there will be bundle metadata with base configuration APKs so that device can identify the minimal APK/Dynamic Features APK to install.

## Benefits:
1.	Google Play uses your app bundle to generate and serve optimized APKs for each device configuration,
2.	Install and Uninstall features on demand or defer to install background
3.	When you use the app bundle format to publish your app, you can also optionally take advantage of Play Feature Delivery, which allows you to add feature modules to your app project.
4.	Publishing with Android App Bundles helps your users to install your app with the smallest downloads possible and increases the compressed download size limit to 150 MB.
5.	Dynamic feature module

![image](https://user-images.githubusercontent.com/11981999/172596909-0467d5b2-0298-482d-9654-9231f2030c1a.png)


Things need to be remembered:
•	Make sure you enable all configuration APKs by setting enableSplit = true for each type of configuration APK. This makes sure that users download only the code and resources they need to run your app on their device.
•	Make sure you shrink your app by removing unused code and resources.
•	Follow best practices to further reduce app size.
•	Consider converting features that are used by only some of your users into feature modules that your app can download later, on demand. Keep in mind, this may require some refactoring of your app, so make sure to first try the other suggestions described above.

## How to Build App Bundle:
There are two ways to create app bundle (AAB):
1.	From Android Studio
2.	From the command line using either gradle or bundletool

### Frome Android Studio:
1.	Need android Android Studio 3.2 or higher
2.	Please go to Build -> Generate Sign APK/Bundle
3.	Select Android App Bundle > Next
4.	Provide the necessary information for the signed bundle like Keystore path, key store password, Key alias, Key password > Click on the “Next Button”
5.	Then need to be select the Build Variants > click on “Finish”
6.	From app > build > output > release/debug/other track we can find the .aab file which can be deployed to the Google Play store.

![image](https://user-images.githubusercontent.com/11981999/172597139-f72a1f98-586b-4125-af34-94519edc7564.png)

## Building App Bundle using Command Line: 
On the command line we can run the bundle task like this:

`./gradlew bundleRelease`

Or for the debug version:

`./gradlew bundleDebug`

Then locate the bundle in your application’s build directory. The default location is `app/build/outputs/bundle/release`.

For the release version we need the signed build for that we can use jarsigner, this is the command to sign the apk:

`jarsigner -keystore $pathToKeystore app-release.aab $keyAlias`

Once the variables are replaced with actual values and the keystore password is entered, the bundle will be signed and ready for upload.

## Uploading through the Play Console
To upload your app bundle to the Play Store, create a new release on a chosen release track. You can drag and drop the bundle into the “App bundles and APKs” section or use the Google Play Developer API.
![image](https://user-images.githubusercontent.com/11981999/172597189-cdf03f36-9eb7-441c-a4a4-d3314b6a4e54.png)

Highlighted (green) section of Play Console for upload of App Bundles.
Once the Bundle is uploaded, the Play Store can optimize the APKs it delivers to users’ de-vices based on their configuration. This in turn reduces download and installation size.

## Exploring your Android App Bundle
To look at how the Play Store ships your app to a user’s device, you can click on the “Details” button at the end of the bundle’s row.
![image](https://user-images.githubusercontent.com/11981999/172597233-6433d877-bea0-40ba-ab35-64d4c8a99f80.png)
Screenshot of the highlighted “Details” button
In the details screen you already see a lot of information on your app bundle such as version code, minSdk level, target SDK, required features, permissions, screen layouts, localizations and much more.
And you also can download signed APKs for your app, to see exactly what the Play Store delivers to a specific device. To navigate there, click on “Explore Bundle” and then open the “Downloads” tab.
You can either select a specific device or apply one or more of the many filters from the “Add filter” tab.

![image](https://user-images.githubusercontent.com/11981999/172599071-f1d602b4-2873-4ff9-ad32-ea25821b7016.png)

Opened filter tab in the app bundle explorer

## Download the app bundle and install locally
In the app bundle explorer, at the end of your screen, there is a “Download” button which provides a zip file, containing several APK, which are tailored to the specific device in ques-tion.
After you download and unzip the file, the containing APK can be installed on a local emula-tor or device by using `adb install — multiple *.apk` from the containing directory.
While each apk in this set is relevant to guarantee correct execution of your app, I want to point out that the base.apk always has to be installed on a device in order to provide your app’s core functionality. Next to code and resources the base module also contains the merged AndroidManifest and shared dependencies for the entire application.
Each feature module or configuration split provides its own resources and can contain code, but the base module is what ties it all together.






