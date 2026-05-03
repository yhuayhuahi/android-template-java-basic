
# Android Template (Java básico)

La idea de esta plantilla es ser liviana. Así puedes usarla como base para crear tu proyecto Android nativo en Java sin tener que configurar todo desde cero.
version: 1.0.1

## Cómo usar esta plantilla

### 0) Requisitos 

Para que `gradlew` funcione necesitas, como mínimo:

- **Java (JDK) 17**: este proyecto usa Android Gradle Plugin `8.2.0`, que requiere Java 17.
	- Verifica: `java -version`
	- Recomendación: Temurin/OpenJDK 17.
	- Si usas `JAVA_HOME`, que apunte al JDK (no al JRE).

- **Android SDK** (Platform + Build Tools): puedes instalarlo con Android Studio (lo más simple) o con Command-line tools.
	- Recomendado instalar al menos:
		- `platforms;android-34`
		- `build-tools;34.0.0` (o el que tengas disponible)
		- `platform-tools`
	- Variables típicas:
		- `ANDROID_SDK_ROOT` (o `ANDROID_HOME`)

- **Gradle**: Es necesario que instales Gradle ya que esta plantilla no incluye el wrapper (`gradlew`). Puedes instalarlo con SDKMAN, Homebrew, o manualmente desde la web de Gradle.
  - Recomendación: Gradle 8.2+ (compatible con AGP 8.2).
  - Verifica: `gradle -v`

Opcional (solo si vas a instalar en un dispositivo/emulador):

- **ADB / emulador** (viene con `platform-tools`).

### 1) Bajar el repo

Clonas este repositorio a en la carpeta en donde deseas crear tu proyecto Android:

```powershell
git clone https://github.com/yhuayhuahi/android-template-java-basic.git
cd android-template-java-basic
```

### 2) Desvincular la relación con el repo original

Para no dejar rastros del repo original, puedes eliminar la carpeta `.git` y luego inicializar un nuevo repositorio Git:

```powershell
Remove-Item -Recurse -Force .git
git init
git add -A
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
git push -u origin main
```

### 3) Probar el funcionamiento de la plantilla

Para comenzar a usar esta plantilla, debes ejecutar lo siguiente:

```powershell
gradle wrapper
```

Esto generará el wrapper de Gradle (`gradlew` y `gradlew.bat`) asi puedes ejecutar Gradle en proyectos Android.


Luego, para compilar y generar el APK de debug (IMPORTANTE: Para instalar en un dispositivo o emulador debes tener conectado un dispositivo o tener un emulador corriendo):

```powershell
./gradlew assembleDebug

./gradlew installDebug
```

## Cómo suele organizarse un proyecto Android nativo (Java)

En esta sección te muestro **cómo suele organizarse un proyecto Android nativo** (Java) para que tengas un mapa mental. Luego ya pasamos a “cómo usar la plantilla”.

### 1) Cómo se organiza un proyecto Android (visión realista)

Un proyecto Android con Gradle normalmente se divide en:

- **Raíz del proyecto**: configuración común, wrapper de Gradle y lista de módulos.
- **Módulos**: `app` (aplicación) y opcionalmente módulos tipo librería (`core`, `feature-*`, `shared`, etc.).
- **Source sets** por módulo: `src/main`, `src/test`, `src/androidTest` y a veces `src/debug`, `src/release`, `src/<flavor>`.

#### Ejemplo de árbol (solo ilustrativo)

```bash
android-template-java-basic/
├── app/                       # Módulo Android Application
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
│   │   ├── debug/              # Código/recursos solo debug (opcional)
│   │   └── release/            # Código/recursos solo release (opcional)
│   ├── proguard-rules.pro      # Si minificas/obfuscación (opcional)
│   └── build.gradle            # Config del módulo app
│
├── core/                       # Ejemplo: módulo Android Library (opcional)
│   └── ...
│
├── build.gradle                # Config raíz (plugins/repos comunes)
├── settings.gradle             # Incluye módulos (app, core, ...)
├── gradle.properties
├── gradlew
├── gradlew.bat
└── gradle/wrapper/
```

### 2) `src/` y los “source sets”

Dentro de un módulo, `src/` suele organizarse así:

- `src/main/`: el código y recursos “base” que van siempre.
- `src/test/`: tests unitarios (corren en JVM, sin Android runtime).
- `src/androidTest/`: tests instrumentados (corren en emulador/dispositivo).
- `src/debug/` y `src/release/`: overrides específicos por tipo de build.
- `src/<flavor>/`: si usas product flavors (por ejemplo `free/`, `paid/`, `dev/`, `prod/`).

En proyectos grandes, esto permite tener manifests/resources/código distintos por variante sin duplicar todo.

### 3) Qué suele haber dentro de `res/` 

Un proyecto Android real casi siempre termina con más carpetas en `res/`. Las más típicas:

- `layout/`: pantallas y componentes en XML.
- `layout-land/`, `layout-sw600dp/`: variantes por orientación/tamaño.
- `values/`: `strings.xml`, `colors.xml`, `dimens.xml`, `styles.xml/themes.xml`.
- `values-es/`, `values-en/`: localización.
- `values-night/`: modo oscuro.
- `drawable/`: imágenes/drawables (shape, vector, selector, etc.).
- `drawable-night/`: drawables alternativos para night.
- `mipmap-*`: íconos de launcher (`mipmap-mdpi`, `mipmap-xhdpi`, etc.).
- `menu/`: menús (Toolbar/Overflow).
- `xml/`: configuraciones varias (por ejemplo `file_paths.xml` para FileProvider, `network_security_config.xml`, etc.).
- `raw/`: archivos “tal cual” accesibles como `R.raw.*`.
- `font/`: fuentes.
- `anim/` y `animator/`: animaciones.
- `navigation/`: gráficos de navegación (si usas Navigation Component).

Importante: Android usa **calificadores** para variantes (`-night`, `-land`, `-sw600dp`, `-es`, etc.). Eso hace que `res/` crezca en proyectos reales.

