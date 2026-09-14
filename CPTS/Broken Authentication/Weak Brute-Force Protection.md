## Rate Limits

This presents an issue, as an attacker can set arbitrary HTTP headers in a request, thereby bypassing the rate limit entirely. This enables an attacker to conduct a brute-force attack by randomizing the `X-Forwarded-For` header in each HTTP request, thereby avoiding the rate limit. Vulnerabilities like this occur frequently in the real world, for instance, as reported in [CVE-2020-35590](https://nvd.nist.gov/vuln/detail/CVE-2020-35590).
## CAPTCHAs

A `Completely Automated Public Turing test to tell Computers and Humans Apart (CAPTCHA)` is a security measure to prevent bots from submitting requests. By forcing humans to make requests instead of bots or scripts, brute-force attacks become a manual task, making them infeasible in most cases. CAPTCHAs typically present challenges that are easy for humans to solve but difficult for bots, such as identifying distorted text, selecting particular objects from images, or solving simple puzzles. By requiring users to complete these challenges before accessing certain features or submitting forms, CAPTCHAs help prevent automated scripts from performing actions that could be harmful, such as spamming forums, creating fake accounts, or launching brute-force attacks on login pages. While CAPTCHAs serve an essential purpose in deterring automated abuse, they can also present usability challenges for some users, particularly those with visual impairments or specific cognitive disabilities.

From a security perspective, it is essential not to reveal a CAPTCHA's solution in the response, as we can see in the following flawed CAPTCHA implementation:

![](Weak%20Brute-Force%20Protection-20260914-091355.png)

Additionally, tools and browser extensions that solve CAPTCHAs automatically are on the rise. Many open-source CAPTCHA solvers are available. In particular, the rise of AI-driven tools provides CAPTCHA-solving capabilities by utilizing powerful image recognition or voice recognition machine learning models.
