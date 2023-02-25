# Topics Included

1.  Review of Common Flaws in Source Code and at Runtime
2.  Modification of App Behavior Through Code/Configuration Changes
3.  Interception of Network Communication aka MitM
4.  Jailbreak/Root Detection Bypasses and App Review from a Privileged Standpoint
5.  Instrumentation (Review and Modification of App Behavior)
6.  CTF Challenges for Attendants to Test Their Skills

**Section 1: Hacking Android & IoT apps by Example**

Part 0 - Android Security Crash Course

## The state of Android Security
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

-   - Android security architecture and its components
https://medium.com/@boshng95/android-security-overview-7386022ad55d
-   - Android apps and the filesystem
https://developer.android.com/training/data-storage
-   - Android app signing, sandboxing and provisioning
https://source.android.com/docs/security/app-sandbox
-   - Recommended lab setup tips
Part 1 - Static Analysis with Runtime Checks

-   - Tools and techniques to retrieve/decompile/reverse and review APKs
https://medium.com/helpshift-engineering/reverse-engineer-your-favorite-android-app-863a797042a6
-   - Identification of the attack surface of Android apps and general information gathering
https://redhuntlabs.com/blog/the-current-state-of-security-privacy-and-attack-surface-on-android-scanning-apps-for-secrets-and-more-wave-8.html
-   - Identification of common vulnerability patterns in Android apps:
    -  + Hardcoded secrets
    https://developer.android.com/topic/security/risks/hardcoded-cryptographic-secrets?hl=es-419
    https://blog.ostorlab.co/hardcoded-secrets.html
    -  + Logic bugs
    https://downloads.immunityinc.com/infiltrate-archives/[Infiltrate]%20Geshev%20and%20Miller%20-%20Logic%20Bug%20Hunting%20in%20Chrome%20on%20Android.pdf
    -  + Access control flaws
    https://onappsec.com/diva-android-access-control-issues/
    -  + Intents
    https://developer.android.com/guide/components/intents-filters?hl=es-419
    -  + Cool injection attacks and more
    https://developer.android.com/topic/security/risks/sql-injection
    https://www.hindawi.com/journals/scn/2018/2489214/
-   - The art of repackaging:
    -  + Tips to get around not having root
    https://www.androidauthority.com/android-hacks-you-can-do-without-rooting-636661/
    -  + Manipulating the Android Manifest
    https://doc.batch.com/cordova/advanced/android-manifest-manipulation/
    - + Defeating SSL/TLS pinning
    https://httptoolkit.com/blog/frida-certificate-pinning/
    - + Defeating root detection
    https://www.youtube.com/watch?v=4X_go9r4nxM
    https://www.youtube.com/watch?v=sXcmJTidzlI
    -  + Dealing with apps in foreign languages and more
    https://developer.android.com/guide/topics/resources/app-languages
Part 2 - Dynamic Analysis

-   - Monitoring data: LogCat, Insecure file storage, Android Keystore, etc.
https://www.androidpolice.com/check-data-use-android-phone-tablet/
-   - The art of MitM: Intercepting Network Communications
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
https://stackoverflow.com/questions/49470807/how-to-elegantly-change-the-programs-behavior-at-runtime
https://httptoolkit.com/blog/android-reverse-engineering/
Part 3 - Test Your Skills

-   - CTF time, including finding IoT vulnerabilities through app analysis

Part 0 - iOS Security Crash Course

-   - The state of iOS Security
https://support.apple.com/guide/security/welcome/web
-   - iOS security architecture and its components
https://study.com/academy/lesson/ios-security-architecture-layers-features.html
-   - iOS app signing, sandboxing and provisioning
https://developer.apple.com/documentation/technotes/tn3125-inside-code-signing-provisioning-profiles
-   - iOS apps and the filesystem
https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html
-   - Recommended lab setup tips

Part 1 - Static Analysis with runtime checks

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

Part 2 - Dynamic Analysis

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

Part 3 - Test your Skills

-   - CTF time, including finding IoT vulnerabilities through app analysis