Once you [turn on Swift Package Manager][], the Flutter CLI tries to migrate
your project to use Swift Package Manager the next time you run your app
using the CLI.

However, the Flutter CLI tool might be unable to migrate your project
automatically if there are unexpected modifications.

If the automatic migration fails, use the steps below to add Swift Package
Manager integration to a project manually.

Before migrating manually, [file an issue][]; this helps the Flutter team
improve the automatic migration process.
Include the error message and, if possible, include a copy of
the following files in your issue:

* `ios/Runner.xcodeproj/project.pbxproj`
* `ios/Runner.xcodeproj/xcshareddata/xcschemes/Runner.xcscheme`
   (or the xcsheme for the flavor used)

### Step 1: Add FlutterGeneratedPluginSwiftPackage Package Dependency {:.no_toc}

1. Open your app (`ios/Runner.xcworkspace`) in Xcode.
2. Navigate to **Package Dependencies** for the project.

   {% render docs/captioned-image.liquid,
   image:"development/packages-and-plugins/swift-package-manager/package-dependencies.png",
   caption:"The project's package dependencies" %}

3. Click <span class="material-symbols-outlined">add</span>.
4. In the dialog that opens, click **Add Local...**.
5. Navigate to `ios/Flutter/ephemeral/Packages/FlutterGeneratedPluginSwiftPackage`
   and click **Add Package**.
6. Ensure that it's added to the `Runner` target and click **Add Package**.
 
   {% render docs/captioned-image.liquid,
   image:"development/packages-and-plugins/swift-package-manager/choose-package-products.png",
   caption:"Ensure that the package is added to the `Runner` target" %}

7. Ensure that `FlutterGeneratedPluginSwiftPackage` was added to **Frameworks,
   Libraries, and Embedded Content**.

   {% render docs/captioned-image.liquid,
   image:"development/packages-and-plugins/swift-package-manager/add-generated-framework.png",
   caption:"Ensure that `FlutterGeneratedPluginSwiftPackage` was added to **Frameworks, Libraries, and Embedded Content**" %}

### Step 2: Add Run Prepare Flutter Framework Script Pre-Action {:.no_toc}

**The following steps must be completed for each flavor.**

1. Go to **Product > Scheme > Edit Scheme**.
2. Expand the **Build** section in the left side bar.
3. Click **Pre-actions**.
4. Click <span class="material-symbols-outlined">add</span> and
   select **New Run Script Action** from the menu.
5. Click the **Run Script** title and change it to:

   ```plaintext
   Run Prepare Flutter Framework Script
   ```

6. Change the **Provide build settings from** to the `Runner` app.
7. Input the following in the text box:

   ```sh
   "$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" prepare
   ```

   {% render docs/captioned-image.liquid,
   image:"development/packages-and-plugins/swift-package-manager/add-flutter-pre-action.png",
   caption:"Add **Run Prepare Flutter Framework Script** build pre-action" %}

### Step 3: Run app {:.no_toc}

1. Run the app in Xcode.
2. Ensure that  **Run Prepare Flutter Framework Script** runs as a pre-action
   and that `FlutterGeneratedPluginSwiftPackage` is a target dependency.

   {% render docs/captioned-image.liquid,
   image:"development/packages-and-plugins/swift-package-manager/flutter-pre-action-build-log.png",
   caption:"Ensure **Run Prepare Flutter Framework Script** runs as a pre-action" %}

3. Ensure that the app runs on the command line with `flutter run`.

[turn on Swift Package Manager]: /packages-and-plugins/swift-package-manager/for-app-developers/#how-to-turn-on-swift-package-manager
[file an issue]: {{site.github}}/flutter/flutter/issues/new?template=2_bug.yml
