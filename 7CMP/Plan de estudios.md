#### Topics Included

1.  Review of Common Flaws in Source Code and at Runtime
2.  Modification of App Behavior Through Code/Configuration Changes
3.  Interception of Network Communication aka MitM
4.  Jailbreak/Root Detection Bypasses and App Review from a Privileged Standpoint
5.  Instrumentation (Review and Modification of App Behavior)
6.  CTF Challenges for Attendants to Test Their Skills

**Section 1: Hacking Android & IoT apps by Example**

Part 0 - Android Security Crash Course

-   - The state of Android Security
-   - Android security architecture and its components
-   - Android apps and the filesystem
-   - Android app signing, sandboxing and provisioning
-   - Recommended lab setup tips
Part 1 - Static Analysis with Runtime Checks

-   - Tools and techniques to retrieve/decompile/reverse and review APKs
-   - Identification of the attack surface of Android apps and general information gathering
-   - Identification of common vulnerability patterns in Android apps:
    1.  + Hardcoded secrets
    2.  + Logic bugs
    3.  + Access control flaws
    4.  + Intents
    5.  + Cool injection attacks and more
-   - The art of repackaging:
    1.  + Tips to get around not having root
    2.  + Manipulating the Android Manifest
    3.  + Defeating SSL/TLS pinning
    4.  + Defeating root detection
    5.  + Dealing with apps in foreign languages and more
Part 2 - Dynamic Analysis

-   - Monitoring data: LogCat, Insecure file storage, Android Keystore, etc.
-   - The art of MitM: Intercepting Network Communications
-   - The art of Instrumentation: Hooking with Xposed
-   - App behaviour monitoring at runtime
-   - Defeating Certificate Pinning and root detection at runtime
-   - Modifying app behaviour at runtime
Part 3 - Test Your Skills

-   - CTF time, including finding IoT vulnerabilities through app analysis

Part 0 - iOS Security Crash Course

-   - The state of iOS Security
-   - iOS security architecture and its components
-   - iOS app signing, sandboxing and provisioning
-   - iOS apps and the filesystem
-   - Recommended lab setup tips

Part 1 - Static Analysis with runtime checks

-   - Tools and techniques to retrieve/decompile/reverse and review IPAs
-   - Identification of the attack surface of iOS apps and general information gathering
-   - Identification of common vulnerability patterns in iOS apps:
    1.  + Hardcoded secrets
    2.  + Logic bugs
    3.  + Access control flaws
    4.  + URL handlers
    5.  + Cool injection attacks, and more
-   - Patching and Resigning iOS binaries to alter app behaviour
-   - Tips to test without a jailbreak

Part 2 - Dynamic Analysis

-   - Monitoring data: caching, logs, app files, insecure file storage, iOS keychain, etc.
-   - Crypto flaws
-   - The art of MitM: Intercepting Network Communications
-   - Defeating certificate pinning and jailbreak detection at runtime
-   - The art of Instrumentation: Introduction to Frida, Objection
-   - App behaviour monitoring at runtime
-   - Modifying app behaviour at runtime

Part 3 - Test your Skills

-   - CTF time, including finding IoT vulnerabilities through app analysis