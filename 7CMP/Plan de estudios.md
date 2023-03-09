# Topics Included

1.  Review of Common Flaws in Source Code and at Runtime
2.  Modification of App Behavior Through Code/Configuration Changes
3.  Interception of Network Communication aka MitM
4.  Jailbreak/Root Detection Bypasses and App Review from a Privileged Standpoint
5.  Instrumentation (Review and Modification of App Behavior)
6.  CTF Challenges for Attendants to Test Their Skills

**Section 1: Hacking Android & IoT apps by Example**

Part 0 - Android Security Crash Course

# The state of Android Security
### App sandbox

En este caso como Android es un sistema operativo basado en linux usa el mismo sistema para isolar la ejecución de una app de las otras al setearle a cada app un UID con un proceso propio.

### App signing

Las firmas son usadas para identificar el autor de una app y actualizarla para no necesitar un sistema complidado y dificil de usar. Todas las apps deben estar siempre firmadas.

### Authentication

Android implementa la pasarela de autenticación de usuario con llaves criptograficas para validar  el almacenamiento y el proveedor del servicio. 

En los dispositivos con lector de huella el usuario puee setear una o más huellas para desbloquear distintas partes del dispositivo y ejecutar otras tareas. En sistemas menos avanzados se puede usar el patron o la contraseña para garantizar un entorno de ejecución seguro.

### Biometrics

A partir de android 9 se implemento una api que permite a los desarrolladores integrar la autenticación biometrica para asegurar el uso de sus apps.

### Encryption

Una vez que el dispositivo es encriptado toda la información del usuario será encriptada antes de ser subida al disco del dispositivo. De esta manera si un actor no autorizado logra romper la seguridad del dispositivo y gana acceso a los datos no podrá leerlos por estar encriptados.

### Keystore

Android implementa un hardware que provee la generación de keys, la importación y exportación de las mismas para usarse en la encriptación y el decifrado asimetrico con modos de margen apropiados.

### Security-Enhanced Linux

Android al ser un OS basado en linux implementa el modelo de SElinux para asegurar el dispositivo y forzar el uso de la MAC como control de acceso a los procesos incluso cuando los procesos estan si estan siendo ejecutados como root o un superusuario. 

### Trusty Trusted Execution Environment (TEE)

Trusty es un OS seguro que ofrece un entorno de ejecución seguro para android. Trusty ejecuta exactamente los mismos procesos que Android pero este los aisla del resto del software e incluso del hardware.

### Verified Boot

La verificación de inicio permite asegurarse de que todo el codigo ejecutado viene de una fuente segura (usualmente dispositivos OEMs), sobre el codigo de un atacante o un archivo corrupto. Esot establece una cadena de confianza completa, empezando por un hardware  protegido de root hasta la partición de inicio.

Source: https://source.android.com/docs/security/features

## Android security architecture and its components

La arquitetura de android hace uso del modelo de aislamiento para contruir el sistema operativo de manera segura. 

### Android Security Model

El modelo de aislamiento y sandbox que usa android permite un entorno seguro a la hora de ejecutar codigo en el dispositivo. Pero esto a su vez es muy limitante ya que restringe a la app de poder acceder a las funcionalidades que no tiene acceso.

Una de las limitaciones se puede dar cuando una app carece de funcionalidades utiles tales como la camara, acceso a la red, servicios de localización, etc. Para evitar estas limitaciones android ha implementado una funcionalidad por IDs que permite que cada funcionalidad permitida se pueda comunicar entre si para y permite habilitar el acceso a archivos criticos del sistema.

### Shared User ID

El uso de IDs de usuario compartido permite que el sistema comparta datos entre aplicaiones. Dichas aplicaciones tienen que estar firmadas con el mismo certificado para que esto pueda asignarsele un ID de usuario compartido. De esta manera los desarrolladores pueden bypassear las restricciones del aislamiento.

### Permission

#### Normal _Permission_

Los permisos normales permiten a la palicación a acceder a funcionalidades a nivel de aplicación. Solo permite acceder a datos y recursos fuera de la aplicación en caso de que lo solicitado sea de menor riesgo. El usuario puede administrar este tipo de permisos son dificultad alguna.

#### Dangerous Permission

Este tipo de permisos le permiten a la app acceder a información privada y incluso a funciones que pueden ser criticas en el sistema Android. Por ejemplo la app con estos permisos puede acceder a la galería, la ubicación, o cualquier otra función que permita ganar acceso a los datos privados del usuario.

#### Signature Permission

Los permisos por firma son los que se le conceden a una app cuando es firmada con el mismo certificado de una app previamente instalada con permisos ya asignados.  Esto es similar al Shared User ID.

https://medium.com/@boshng95/android-security-overview-7386022ad55d

## Android apps and the filesystem

Android usa un sistema similar al de un PC en la manera en la que distribuye los archivos en el disco. Por ende, sigue esta estructura:

- **App-specific storage**: Esta parte del almacenamiento guarda todas la información necesaria para que las aplicaciones puedan funcionar correctamente. De esta manera evita que las apps sin permisos adecuados puedan acceder a información privada almacenada en otras partes del disco.
- **Shared storage**: Aquí se almacenan los archivos que las apps se comparten mutuamente para su uso.
- **Preferences**: Almacenamiento privado, para guardar información como los pares de llaves.
- **Databases**: Aquí se genera una base de datos privadas para estructurar datos usando la librearía Room.

| | Type of content | Access method | Permissions needed | Can other apps access? | Files removed on app uninstall? | 
| -- | -- | -- | -- | -- | -- |
| [App-specific files](https://developer.android.com/training/data-storage/app-specific) | Files meant for your app's use only | From internal storage, `getFilesDir()` or `getCacheDir()` From external storage, `getExternalFilesDir()` or `getExternalCacheDir()` | Never needed for internal storage Not needed for external storage when your app is used on devices that run Android 4.4 (API level 19) or higher | No | Yes |
| [Media](https://developer.android.com/training/data-storage/shared/media) | Shareable media files (images, audio files, videos) | `MediaStore` API | `READ_EXTERNAL_STORAGE` when accessing other apps' files on Android 11 (API level 30) or higher `READ_EXTERNAL_STORAGE` or `WRITE_EXTERNAL_STORAGE` when accessing other apps' files on Android 10 (API level 29) Permissions are required for **all** files on Android 9 (API level 28) or lower | Yes, though the other app needs the `READ_EXTERNAL_STORAGE` permission | No |
| [Documents and other files](https://developer.android.com/training/data-storage/shared/documents-files) | Other types of shareable content, including downloaded files | Storage Access Framework | None | Yes, through the system file picker | No |
| [App preferences](https://developer.android.com/training/data-storage/shared-preferences) | Key-value pairs | [Jetpack Preferences](https://developer.android.com/guide/topics/ui/settings/use-saved-values) library | None | No | Yes |
| Database | Structured data | [Room](https://developer.android.com/training/data-storage/room) persistence library | None | No | Yes |

### Categories of storage locations

Android provee dos formas de almacenar la información: el almacenamiento interno y el almacenamiento externo. En la mayoría de los dispositivos el almacenamiento interno es menor al externo, pero el almacenamiento externo no siempre esta disponible. Es por eso que las aplicaciones usan el almacenamiento interno para almacenar la información y los datos de las apps instaladas.

En cualquier caso, esto se puede cambiar al modificar esta linea en el manifiesto del APK:

```xml
<manifest ...  **android:installLocation="preferExternal"**>  ...</manifest>
```

### Permissions and access to external storage

Android define los siguientes permisos en el almacenamiento: [`READ_EXTERNAL_STORAGE`](https://developer.android.com/reference/android/Manifest.permission#READ_EXTERNAL_STORAGE), [`WRITE_EXTERNAL_STORAGE`](https://developer.android.com/reference/android/Manifest.permission#WRITE_EXTERNAL_STORAGE), y [`MANAGE_EXTERNAL_STORAGE`](https://developer.android.com/reference/android/Manifest.permission#MANAGE_EXTERNAL_STORAGE).

En las versiones más antiguas de android hacía falta definir el READ_EXTERNAL_STORAGE para que las apps pudieran acceder a los archivos del sistema. Pero en las versiones más actuales, Android se fija más en el hecho de la ubicación del archivo que en la configuración de los permisos para mostrar un archivo a un APK, al punto de que en Android 11 ya los permisos directamente no afectan en nada en esto.

### Scoped storage

Este es un tipo de almacenamiento que esta disponible a partir de android 10 y esta pensado para otorgarle cierto control a las apps de usar el almacenamiento externo por default. Eso si, estas apps solo pueden acceder a determinadas locaciones del almacenamiento externo.

https://developer.android.com/training/data-storage
## Android app signing, sandboxing and provisioning

Android aprovecha la protección basada en usuario de Linux, ya que es un sistema basado en el mismo. De esta manera aisla la aplicación y los recursos que esta usando y evita que se puedan acceder de manera externa.

Como este sistema de seguridad nace desde el kernel, esto permite que el modelo se pueda extender tanto a la aplicación como al sistema operativo en si mismo. Esto significa que absolutamente todo lo que necesita el sistema operativo para funcionar (y lo que no) es lanzado usando el sandbox.

### Protections

Generalmente, para poder sacar una aplicacación del sandbox en un dispositivo bien configurado no es muy fácil. Pero como siempre ocurre, según el modelo y la versión de Android pueden existir maneras de bypassear la protección del Sandbox.

-   In Android 5.0, SELinux provided mandatory access control (MAC) separation between the system and apps. However, all third-party apps ran within the same SELinux context so inter-app isolation was primarily enforced by UID DAC.
-   In Android 6.0, the SELinux sandbox was extended to isolate apps across the per-physical-user boundary. In addition, Android also set safer defaults for application data: For apps with `targetSdkVersion >= 24`, default DAC permissions on an app's home dir changed from 751 to 700. This provided safer default for private app data (although apps may override these defaults).
-   In Android 8.0, all apps were set to run with a `seccomp-bpf` filter that limited the syscalls that apps were allowed to use, thus strengthening the app/kernel boundary.
-   In Android 9 all non-privileged apps with `targetSdkVersion >= 28` must run in individual SELinux sandboxes, providing MAC on a per-app basis. This protection improves app separation, prevents overriding safe defaults, and (most significantly) prevents apps from making their data world accessible.
-   In Android 10 apps have a limited raw view of the filesystem, with no direct access to paths like /sdcard/DCIM. However, apps retain full raw access to their package-specific paths, as returned by any applicable methods, such as [Context.getExternalFilesDir()](https://developer.android.com/reference/android/content/Context.html#getExternalFilesDir(jav%20a.lang.String)).

### Guidelines for sharing files

El almacenamiento se puede setear para que sea accesible para todo el mundo, pero esto no es una buena practica ya que no se puede limitar a que solo pueda acceder la aplicación que deseamos. Esta practica ha conllevado a que se produzcan information disclousures y information leakages con los cuales se han logrado escalar a vulnerabilidades mayores.

En Android 9 y versiones más nuevas, compartir archivos de esta manera ya no esta permitido en las aplicaciones que tengan seteado `targetSdkVersion>=28`

- Si la aplicación necesita compartir información o datos con otra app una manera de lograr esto es con un proveedor de contenido. De esta manera se pueden compartir datos con los permisos adecuados y las restricciones adecuadas ya que se estaría usando los permisos que se usan en los sistemas UNIX.
- Si la aplicación tiene datos que deben ser compartidos con todo el sistema, deben ser datos de tipo media (fotos, videos y audios) y además de eso debe  estar guardados en el MediaStorage para que se puedan acceder a ellos de manera segura.

https://source.android.com/docs/security/app-sandbox
-   - Recommended lab setup tips
# Part 1 - Static Analysis with Runtime Checks

## Tools and techniques to retrieve/decompile/reverse and review APKs

- Apktool
- Dex2jar
- JD-GUI
Para empezar a decompilar se debe hacer lo siguiente:

- Primero se debe dercargar el apk.
- Luego se debe ejecutar lo siguiente en la terminal

```bash
apktool d --no-src <apk name>
```

- Ejecutando el comando de arriba se puede decompilar el APK en un archivo que pueda ser legible por un humano y también se puede obtener el XML del APK. Todos los archivos terminan convirtiendose en sus archivos originales.
### Obteniendo el codigo fuente

El codigo se puede obtener a traves de los archivos .dex. Estos archivos se pueden convertir a archivos jar usando dex2jar que es una herramienta que decompila los archivos dex para obtener el archivo jar orginal.

- Para lograr esto primero se debe tener una copia de dex2jar en la carpeta donde decompilamos el APK
- Tenemos que crear la carpeta "Dex2jar" en la carpeta anterior a donde decompilamos el apk. Luego debemos ejecutar los siguientes comandos `chmod u+x d2j_invoke.sh` y `chmod u+x d2j-dex2jar.sh` para habilitar la ejecución del dex2jar.
- Ahora ejecuta `/d2j-dex2jar.sh <apk-name>_classes.dex` en la carpeta dex2jar. Esto va a crear el archivo `<apk-name>_classes-dex2jar.jar` dentro de la carpeta dex2jar

Ya con esto listo se puede usar JD-GUI para ver como funcionaría el codigo:

- Abre JD-GUI usando las intrucciones de este [link](https://github.com/java-decompiler/jd-gui)
- Usando JD-GUI se debe abrir el `<apk-name>_classes-dex2jar.jar` que esta en la carpeta dex2jar.

Ya con esto se podrá analizar el codigo fuente de cualquier aplicación sin mucha complicación.

https://medium.com/helpshift-engineering/reverse-engineer-your-favorite-android-app-863a797042a6
## Identification of the attack surface of Android apps and general information gathering
https://redhuntlabs.com/blog/the-current-state-of-security-privacy-and-attack-surface-on-android-scanning-apps-for-secrets-and-more-wave-8.html
## Identification of common vulnerability patterns in Android apps:
- Hardcoded secrets
Este ataque se da cuando los desarrolladores dejan información sensible de manera codificada en la app que se podría decodificar con ingeniería inversa o con conocimientos criptograficos.
https://developer.android.com/topic/security/risks/hardcoded-cryptographic-secrets?hl=es-419
https://blog.ostorlab.co/hardcoded-secrets.html
- Logic bugs
Este tipo de bugs son los que se generan cuando un atacante exploitea la logica del negocio para causar un comportamiento dañino para el mismo. Esto ocurre cuando se exploitean funcionalidades que en un principio son seguras pero que si se usan de determinada manera pueden ser maliciosas.
https://downloads.immunityinc.com/infiltrate-archives/[Infiltrate]%20Geshev%20and%20Miller%20-%20Logic%20Bug%20Hunting%20in%20Chrome%20on%20Android.pdf
-  Access control flaws
https://onappsec.com/diva-android-access-control-issues/
-  Intents
Un intent es un objeto de mensajería que se puede usar para solicitar una acción en otro componente de la app. Estos pueden iniciar un servicio, iniciar una actividad o tranmitir una emisión.

Existen dos tipos, la intent implicita y la explicita. La primera no declara el componente pero si declara una acción en general; en cambio la implicita declara el componente y demás información relacionada.
https://developer.android.com/guide/components/intents-filters?hl=es-419
- Cool injection attacks and more
SQLi:

Similar a las SQLi en web, en android se pueden conseguir al manipular el input de un parametro para que este reciba una query SQL maliciosa. Aunque también existen las vulnerabilidades SQL por los content provider (algo que no existen en las SQLi en webs), las cuales se pueden dar en dos casos:
1. Que multiples content providers compartan el acceso a una base de datos sqlite, lo que significa que si uno de los content providers es vulnerado podrá ganar acceso a todos los demás
2. Un content provider tiene muchos permisos a la hora de escribir información en una base de datos, lo que podría usarse para ganar permisos y acceso a partes donde en un inicio no se podría acceder.
 https://developer.android.com/topic/security/risks/sql-injection
 
## The art of repackaging:
- Tips to get around not having root
https://www.androidauthority.com/android-hacks-you-can-do-without-rooting-636661/
- Manipulating the Android Manifest
Para manipular el manifiesto se debe hacer lo siguiente:
1. Abrir el `config.xml` y irse hasta donde dice `<platform name="android">`
2. Allí se debe añadir lo siguiente:
```xml
<platform name="android">
  <config-file target="AndroidManifest.xml" parent="/manifest/application">
   <!-- Your manifest edits go here. These lines will be added in AndroidManifest.xml's <application> tag. -->
  </config-file>
</platform>
```
3. Y ya se podrá añadir la modificación en la parte en la que se encuentra el comentario.
https://doc.batch.com/cordova/advanced/android-manifest-manipulation/
- Defeating SSL/TLS pinning
Esta es una practica de seguridad en la que se checkea el certificado de la conexión HTTPS que se esta realizando. De esta manera se evita la mayoría de los ataques de tipo MITM. Aunque esta protección se puede quitar usando Frida de la siguiente manera:
1. Se debe conectar ADB a un dispositivo que este rooteado.
2. Se debe instalar y iniciar Frida en el dispositivo.
3. Se debe instalar e iniciar Frida en el PC
4. Se debe seleccionar cual es la aplicación que se quiere modificar
Para los pasos detallados se puede seguir la guía que aparece en la fuente.
https://httptoolkit.com/blog/frida-certificate-pinning/
- Defeating root detection
https://www.youtube.com/watch?v=4X_go9r4nxM
https://www.youtube.com/watch?v=sXcmJTidzlI
- Dealing with apps in foreign languages and more
https://developer.android.com/guide/topics/resources/app-languages
# Part 2 - Dynamic Analysis

-   - Monitoring data: LogCat, Insecure file storage, Android Keystore, etc.
1. **Logcat**: Una manera de ver los registros de una app es usando ADB que es una aplicación que se especializa en debuggear apps de android. Si se escribe `adb logcat` en la terminal podremos acceder a estos registros.
2. **Insecure file storage**: Para chequear si esta vulnerabilidad esta en la app podremos ejecutar en la terminal `adb shell` donde podrémos ganar una consola en el dispositivo. Luego usando `ls -l /data/data/b3nac.injuredandroid/files` en el directorio de la app podremos revisar si estan bien seteados los permisos. Luego usando `adb -s 192.168.0.0 pull /data/data/b3nac.injuredandroid/files/<file> /root/Desktop` se podrá traer al PC el archivo en especifico.
3. El Keystore es un contenedor ene el almacenamiento que se encarga de almacenar todas las llaves criptograficas usadas por las aplicaciones ya sea para autenticarse, comunicarse o sencillamente encriptar su información.
https://www.xatakandroid.com/programacion-android/logcat-android-que-como-ver-este-registro-mensajes-sistema
https://pallabjyoti218.medium.com/android-insecure-file-storage-e770eed9a3fa
-   - The art of MitM: Intercepting Network Communications
Existen distintas herramientas para esto, pero la más famosa de todas es BurpSuite que no es más que un proxy que se puede configurar para analizar cada petición realizada por una app.
https://approov.io/blog/how-to-mitm-attack-the-api-of-an-android-app
-   - The art of Instrumentation: Hooking with Xposed
https://resources.infosecinstitute.com/topic/android-hacking-and-security-part-25-hooking-and-patching-android-apps-using-xposed-framework/
-   - App behaviour monitoring at runtime
https://developer.android.com/guide/practices/verifying-apps-art
https://www.igi-global.com/article/a-privacy-protection-approach-based-on-android-applications-runtime-behavior-monitor-and-control/205526
-   - Defeating Certificate Pinning and root detection at runtime
https://www.netspi.com/blog/technical/mobile-application-penetration-testing/four-ways-bypass-android-ssl-verification-certificate-pinning/
https://medium.com/@click4abhishekagarwal/pin-there-done-that-93033a351354
-   - Modifying app behaviour at runtime
Este es un metodo de asegurar que no se estan usando certificados de terceros en la comunicación, lo que puede evitar o prevenir cietrto tipo de ataques (sobre todo de tipo MITM). Una de las maneras de evitar esto es usando Frida (aunque hay opciones como Xposed o Objection)
https://stackoverflow.com/questions/49470807/how-to-elegantly-change-the-programs-behavior-at-runtime
https://httptoolkit.com/blog/android-reverse-engineering/
# Part 3 - Test Your Skills

-   - CTF time, including finding IoT vulnerabilities through app analysis

# Part 0 - iOS Security Crash Course

## The state of iOS Security
Apple es una de las compañías de moviles que más se ha enfocado en segurizar sus dispositivos, por lo que siempre tratan de mantener al días sus scanners de seguridad para evitar que aplicaciones con malware o con cambios sean ejecutadas en sus dispositivos.
https://support.apple.com/guide/security/welcome/web

## iOS security architecture and its components
La seguridad en IOS se divide en dos partes. Por un lado esta la protección implementada a nivel de software y, por otro, esta las protecciones implementadas a nivel de hardware.

### Software:
- Las aplicaciones funcionan en un sandbox
- Los directorios principales del sistema estan separados en particiones distintas (una para el Home y otra para los archivos del OS).
- La protección de datos por permisos que permite saber si un dato se puede acceder desde una aplicación o no.
- Y un sistema de archivos que permite almacenar los archivos necesarios para el sistema.

### Hardware:
- Los componentes del kernel del dispositivo estan estrictamente protegidos.
- Un sistema que se encarga de almacenar la información de inicio del telefono (tales como el Touch ID).
- Un chequeo de seguridad fisico a la hora de hacer pagos para optimizar la seguridad.
- Un sistema de encriptado que se encarga de encriptar y deseincriptar archivos del sistema.
- Las llaves de seguridad que son de confianza para el sistema.
https://study.com/academy/lesson/ios-security-architecture-layers-features.html

## iOS app signing, sandboxing and provisioning


https://developer.apple.com/documentation/technotes/tn3125-inside-code-signing-provisioning-profiles

-   - iOS apps and the filesystem
https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html
-   - Recommended lab setup tips

# Part 1 - Static Analysis with runtime checks

-   - Tools and techniques to retrieve/decompile/reverse and review IPAs

https://www.corellium.com/blog/ios-mobile-reverse-engineering
https://www.apriorit.com/dev-blog/363-how-to-reverse-engineer-os-x-and-ios-software
-   - Identification of the attack surface of iOS apps and general information gathering
https://mas.owasp.org/MASTG/iOS/0x06a-Platform-Overview/
https://typhooncon.com/hacking-android-ios-and-iot-apps-by-example/
-   - Identification of common vulnerability patterns in iOS apps:
    -  + Hardcoded secrets
    https://developer.apple.com/forums/thread/110033
    https://www.netguru.com/blog/hardcoded-keys-storage-mobile-app
    -  + Logic bugs
    https://a2nkf.github.io/unauthd_Logic_bugs_FTW/
    https://www.youtube.com/watch?v=oAMZxKsZQp0
    -  + Access control flaws
    https://www.wired.co.uk/article/apple-ios-google-chrome-android-security-patches-august-2022
    https://mashable.com/article/apple-security-flaw-hackers
    -  + URL handlers
    https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app
    -  + Cool injection attacks, and more
    https://developer.apple.com/library/archive/documentation/Security/Conceptual/SecureCodingGuide/OtherHardeningTechniques/OtherHardeningTechniques.html
-   - Patching and Resigning iOS binaries to alter app behaviour
https://labs.withsecure.com/publications/repacking-and-resigning-ios-applications
https://github.com/OWASP/owasp-mastg/blob/master/Document/0x06c-Reverse-Engineering-and-Tampering.md
-   - Tips to test without a jailbreak
https://medium.com/securing/pentesting-ios-apps-without-jailbreak-91809d23f64e
https://www.youtube.com/watch?v=Ehd69t7S44I
https://developer.apple.com/forums/thread/692391

# Part 2 - Dynamic Analysis

-   - Monitoring data: caching, logs, app files, insecure file storage, iOS keychain, etc.
https://www.kodeco.com/20952676-monitoring-for-ios-with-metrickit-getting-started
https://medium.com/expedia-group-tech/monitoring-app-performance-on-ios-7a48fb25cfc2
-   - Crypto flaws
https://crypto-cards.io/basics-of-ios-application-penetration-testing/
https://www.veracode.com/security/insecure-crypto
-   - The art of MitM: Intercepting Network Communications
https://automationhacks.io/2018/09/07/viewing-network-traffic-calls-on-ios-real-device-using-mitm-proxy/
https://www.trickster.dev/post/setting-up-mitmproxy-with-ios15/
-   - Defeating certificate pinning and jailbreak detection at runtime
https://www.appknox.com/blog/bypass-ssl-pinning-in-ios-app
-   - The art of Instrumentation: Introduction to Frida, Objection
https://frida.re/docs/ios/
https://book.hacktricks.xyz/mobile-pentesting/ios-pentesting/frida-configuration-in-ios
https://www.virtuesecurity.com/kb/ios-frida-objection-pentesting-cheat-sheet/
-   - App behaviour monitoring at runtime
https://developer.apple.com/documentation/xcode/diagnosing-and-resolving-bugs-in-your-running-app
https://svitla.com/blog/mobile-application-performance-monitoring-tools-for-ios
-   - Modifying app behaviour at runtime
https://www.youtube.com/watch?v=5AZhfBAVgLs
https://www.youtube.com/watch?v=UlmataDRLx0
https://mobile-security.gitbook.io/mobile-security-testing-guide/ios-testing-guide/0x06c-reverse-engineering-and-tampering

# Part 3 - Test your Skills

-   - CTF time, including finding IoT vulnerabilities through app analysis