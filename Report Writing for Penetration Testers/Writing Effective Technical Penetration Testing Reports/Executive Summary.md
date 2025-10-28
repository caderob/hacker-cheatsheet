# Executive Summary

Pertinent Details
>``` shell
>"Executive Summary:
>
>- Scope: https://kali.org/login.php
>- Timeframe: Jan 3 - 5, 2022
>- OWASP/PCI Testing methodology was used
>- Social engineering and DoS testing were not in scope
>- No testing accounts were given; testing was black box from an external IP address
>- All tests were run from 192.168.1.2"
>```

Describing the Engagement
>``` shell
>"The Client hired OffSec to conduct a penetration test of
>their kali.org web application in October of 2025. The test was conducted
>from a remote IP between the hours of 9 AM and 5 PM, with no users
>provided by the Client."
>```

Identifying the positives
>``` shell
>"The application had many forms of hardening in place. First, OffSec was unable to upload malicious files due to the strong filtering
>in place. OffSec was also unable to brute force user accounts
>because of the robust lockout policy in place. Finally, the strong
>password policy made trivial password attacks unlikely to succeed.
>This points to a commendable culture of user account protections."
>```

Explaining a vulnerability
>``` shell
>"However, there were still areas of concern within the application.
>OffSec was able to inject arbitrary JavaScript into the browser of
>an unwitting victim that would then be run in the context of that
>victim. In conjunction with the username enumeration on the login
>field, there seems to be a trend of unsanitized user input compounded
>by verbose error messages being returned to the user. This can lead
>to some impactful issues, such as password or session stealing. It is
>recommended that all input and error messages that are returned to the
>user be sanitized and made generic to prevent this class of issue from
>cropping up."
>```

Concise conclusion
>``` shell
>"These vulnerabilities and their remediations are described in more
>detail below. Should any questions arise, OffSec is happy
>to provide further advice and remediation help."
>```
