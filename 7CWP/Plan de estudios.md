
## Temario:

1.  Review of Common Flaws in Source Code and at Runtime
https://pentesterlab.com/exercises/codereview/course

2. Web - Interception of Network Communication and MitM-proxy techniques to find security flaws in these platforms
https://cheapsslsecurity.com/blog/types-of-man-in-the-middle-attacks/

3.  Platform-specific attack vectors against Modern Web apps & mitigation
https://www.balbix.com/insights/attack-vectors-and-breach-methods/

4.  CTF Challenges for Attendants to Test Their Skills
https://0xdf.gitlab.io/2022/10/08/htb-opensource.html
https://systemweakness.com/hack-the-box-htb-updown-walkthrough-940cf677cdc


**Section 1: Hacking Modern Web apps by Example**

Part 0 - Modern Web Apps Security Crash Course
-   - The state of Modern Web Apps Security
https://www.codica.com/blog/web-application-security/
-   - Modern Web Apps architecture
https://mobidev.biz/blog/web-application-architecture-types
-   - Introduction to Modern Web Apps
https://medium.com/javarevisited/intro-to-modern-web-development-d714563c87e
-   - Modern Web Apps the filesystem
https://www.altamira.ai/blog/how-does-web-apps-work-with-local-files-through-the-browser/
-   - JavaScript prototypes
https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes
https://www.programiz.com/javascript/prototype
-   - Recommended lab setup tips

Part 1 - Static Analysis, Modern Web Apps frameworks and Tools
-   - Modern Web Apps frameworks and their components
https://www.netsolutions.com/insights/top-web-development-frameworks/
-   - Finding vulnerabilities in Modern Web Apps dependencies
https://www.youtube.com/watch?v=c1yMK_XYqGc
https://techbeacon.com/app-dev-testing/13-tools-checking-security-risk-open-source-dependencies
-   - Common misconfigurations / flaws in Modern Web Apps applications andframeworks
https://relevant.software/blog/web-application-security-vulnerabilities/
-   - Tools and techniques to find security flaws in Modern Web Apps
https://snyk.io/learn/application-security/web-application-security/
https://brightsec.com/blog/web-application-vulnerabilities/
https://resources.infosecinstitute.com/topic/14-popular-web-application-vulnerability-scanners/

Part 2 - Finding and fixing Modern Web Apps vulnerabilities
-   - Identification of the attack surface of Modern Web Apps and general information ngathering
https://www.techtarget.com/whatis/definition/attack-surface
-   - Identification of common vulnerability patterns in Modern Web Apps:
    1.  + CSRF
    https://portswigger.net/web-security/csrf
    2.  + XSS
    https://portswigger.net/web-security/cross-site-scripting
    3.  + Access control flaws
    https://portswigger.net/web-security/access-control
    4.  + NOSQL Injection, MongoDB attacks
    
    5.  + SQL Injection
    https://portswigger.net/web-security/sql-injection
    6.  + RCE
    https://portswigger.net/web-security/os-command-injection
    10.  + Crypto
-   - Monitoring data: Logs, Insecure file storage, etc

Part 3 - Test your Skills
- CTF time
  
**Section 2: Advanced Modern Web Apps attacks**

Part 0 - Advanced Attacks on Modern Web Apps
-   - Leaking data from memory at runtime
https://codete.com/blog/what-are-memory-leaks-and-how-to-detect-them
https://www.mono-project.com/docs/advanced/runtime/memory-leaks/
-   - Prototype Pollution Attack
https://portswigger.net/web-security/prototype-pollution
-   - From deserialization to RCE
https://portswigger.net/web-security/deserialization
-   - Server Side Template Injection
https://portswigger.net/web-security/server-side-template-injection
-   - OAuth attacks
https://portswigger.net/web-security/oauth
-   - JWT attacks
https://portswigger.net/web-security/jwt
-   - Scenarios with CSP
https://portswigger.net/web-security/cross-site-scripting/content-security-policy
-   - Scenarios with Angular.js
https://portswigger.net/research/xss-without-html-client-side-template-injection-with-angularjs
-   - Race conditions
https://krevetk0.medium.com/how-to-check-race-conditions-in-web-applications-338f73937992
-   - Sandbox related security
https://www.forcepoint.com/cyber-edu/sandbox-security
-   - Real world case studies

Part 1 - Advanced Modern Web Apps CTF
- Challenges to practice advanced attacks