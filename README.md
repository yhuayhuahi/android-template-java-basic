
# Android Template (Basic Java)

The goal of this template is to stay lightweight, so you can use it as a base to create a native Android project in Java without configuring everything from scratch.
_version_: 1.0.2

## How to use this template

### 0) Requirements

For `gradlew` to work you need, at minimum:

- **Java (JDK) 17**: this project uses Android Gradle Plugin `8.2.0`, which requires Java 17.
	- Check: `java -version`
	- Recommendation: Temurin/OpenJDK 17.
	- If you use `JAVA_HOME`, make sure it points to the JDK (not the JRE).

- **Android SDK** (Platform + Build Tools): you can install it with Android Studio (easiest) or with the command-line tools.
	- Recommended to install at least:
		- `platforms;android-34`
		- `build-tools;34.0.0` (or whichever version you have available)
		- `platform-tools`
	- Typical environment variables:
		- `ANDROID_SDK_ROOT` (or `ANDROID_HOME`)

- **Gradle**: you must install Gradle because this template does not include the wrapper (`gradlew`). You can install it with SDKMAN, Homebrew, or manually from the Gradle website.
  - Recommendation: Gradle 8.2+ (compatible with AGP 8.2).
  - Check: `gradle -v`

Optional (only if you will install to a device/emulator):

- **ADB / emulator** (comes with `platform-tools`).

### 1) Download the repo

Clone this repository into the folder where you want to create your Android project:

```powershell
git clone https://github.com/yhuayhuahi/android-template-java-basic.git
cd android-template-java-basic
```

### 2) Unlink from the original repository

To avoid leaving traces of the original repo, you can delete the `.git` folder and then initialize a new Git repository:

```powershell
Remove-Item -Recurse -Force .git
git init
git add -A
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USER/YOUR_REPO.git
git push -u origin main
```

### 3) Test that the template works

To start using this template, run:

```powershell
gradle wrapper
```

This will generate the Gradle wrapper (`gradlew` and `gradlew.bat`) so you can run Gradle in Android projects.

Then, to build and generate the debug APK (IMPORTANT: to install on a device or emulator you must have a device connected or an emulator running):

```powershell
./gradlew assembleDebug

./gradlew installDebug
```

## How a native Android project is usually organized (Java)

In this section I show you **how a native Android project is usually organized** (Java) so you have a mental map.

### 1) How an Android project is structured (realistic view)

An Android project with Gradle is usually split into:

- **Project root**: shared configuration, Gradle wrapper, and the list of modules.
- **Modules**: `app` (application) and optionally library modules (`core`, `feature-*`, `shared`, etc.).
- **Source sets** per module: `src/main`, `src/test`, `src/androidTest` and sometimes `src/debug`, `src/release`, `src/<flavor>`.

#### Example tree (illustrative only)

```bash
android-template-java-basic/
├── app/                       # Android Application module
│   ├── src/
│   │   ├── main/
│   │   │   ├── AndroidManifest.xml
│   │   │   ├── java/
│   │   │   │   └── com/example/app/...
│   │   │   ├── res/
│   │   │   │   ├── layout/
│   │   │   │   ├── values/
│   │   │   │   ├── drawable/
│   │   │   │   └── ...
│   │   │   └── assets/
│   │   ├── test/               # Unit tests (JVM)
│   │   ├── androidTest/         # Instrumentation tests (device/emulator)
│   │   ├── debug/              # Debug-only code/resources (optional)
│   │   └── release/            # Release-only code/resources (optional)
│   ├── proguard-rules.pro      # If you minify/obfuscate (optional)
│   └── build.gradle            # app module configuration
│
├── core/                       # Example: Android Library module (optional)
│   └── ...
│
├── build.gradle                # Root config (common plugins/repos)
├── settings.gradle             # Includes modules (app, core, ...)
├── gradle.properties
├── gradlew
├── gradlew.bat
└── gradle/wrapper/
```

### 2) `src/` and “source sets”

Inside a module, `src/` is usually organized like this:

- `src/main/`: the “base” code and resources that always ship.
- `src/test/`: unit tests (run on the JVM, without the Android runtime).
- `src/androidTest/`: instrumentation tests (run on an emulator/device).
- `src/debug/` and `src/release/`: build-type-specific overrides.
- `src/<flavor>/`: if you use product flavors (for example `free/`, `paid/`, `dev/`, `prod/`).

In larger projects this enables different manifests/resources/code per variant without duplicating everything.

### 3) What you usually find inside `res/`

A real Android project almost always ends up with more folders inside `res/`. The most common ones:

- `layout/`: screens and UI components in XML.
- `layout-land/`, `layout-sw600dp/`: variants by orientation/screen size.
- `values/`: `strings.xml`, `colors.xml`, `dimens.xml`, `styles.xml/themes.xml`.
- `values-es/`, `values-en/`: localization.
- `values-night/`: dark mode.
- `drawable/`: images/drawables (shape, vector, selector, etc.).
- `drawable-night/`: alternate drawables for night mode.
- `mipmap-*`: launcher icons (`mipmap-mdpi`, `mipmap-xhdpi`, etc.).
- `menu/`: menus (Toolbar/Overflow).
- `xml/`: miscellaneous configurations (for example `file_paths.xml` for FileProvider, `network_security_config.xml`, etc.).
- `raw/`: “as-is” files accessible as `R.raw.*`.
- `font/`: fonts.
- `anim/` and `animator/`: animations.
- `navigation/`: navigation graphs (if you use Navigation Component).

Important: Android uses **resource qualifiers** for variants (`-night`, `-land`, `-sw600dp`, `-es`, etc.). That’s why `res/` grows in real projects.

