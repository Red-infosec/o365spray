# o365spray

This is a basic username enumeration and password spraying tool aimed at Microsoft O365. For educational purposes only.

Microsoft makes it possible to identify valid and invalid usernames when the domain is using O365. If an account on a non-O365 domain is attempted Microsoft will not flag this as an invalid username (404 HTTP response), but instead return responses making it seem as if the account is is valid and the password was incorrect. This allows for false positives during enumeration so this tool includes the `--validate` flag in order to identify if a specific domain is using Microsoft O365 prior to performing spraying/enumeration.

The best way to use this script is to:
1. Validate any and all domains
2. Enumerate potential users
3. Spray valid users

NOTE: User enumeration is performed by submitting a single authentication attempt per user. If user enumeration is performed prior to password spraying please consider that all valid users have already submitted a single authentication attempt.

## Usage
Validate a domain is using O365:<br>
`python3 o365spray.py --validate --domain test.com`

Perform username enumeration:<br>
`python3 o365spray.py --enum -U usernames.txt --domain test.com`

Perform password spray:<br>
`python3 o365spray.py --spray -U usernames.txt -P passwords.txt --count 2 --lockout 5 --domain test.com`


```
usage: o365spray.py [-h] [-v] [-e] [-s] -d DOMAIN [-u USERNAME] [-p PASSWORD]
                    [-U USERNAMES] [-P PASSWORDS] [-c COUNT] [-l LOCKOUT]
                    [--proxy PROXY] [--threads THREADS] [--output OUTPUT]
                    [--paired] [--debug] [--spray-secondary]
                    [--validate-secondary]

Microsoft O365 User Enumerator and Password Sprayer

optional arguments:
  -h, --help            show this help message and exit
  -v, --validate        Validate a domain is running O365
  -e, --enum            Perform username enumeration
  -s, --spray           Perform password spraying
  -d DOMAIN, --domain DOMAIN
                        Domain name to validate against O365
  -u USERNAME, --username USERNAME
                        Username(s) delimited using commas
  -p PASSWORD, --password PASSWORD
                        Password(s) delimited using commas
  -U USERNAMES, --usernames USERNAMES
                        File containing list of usernames
  -P PASSWORDS, --passwords PASSWORDS
                        File containing list of passwords
  -c COUNT, --count COUNT
                        Number of password attempts to run before resetting
                        lockout timer. Default: 1
  -l LOCKOUT, --lockout LOCKOUT
                        Lockout policy reset time (in minutes). Default: 5
                        minutes
  --proxy PROXY         Proxy to pass traffic through: <ip:port>
  --threads THREADS     Number of threads to run. Default: 10
  --output OUTPUT       Output file name for enumeration and spraying
  --paired              Password spray pairing usernames and passwords (1:1).
  --debug               Debug output
  --spray-secondary     Use `ActiveSync` for password spraying instead of
                        `Autodiscover`
  --validate-secondary  Use `openid-configuration` for domain validation
                        instead of `getuserrealm`
```

## Acknowledgments

**grimhacker** - *Research and discovery of user enumeration within Office 365 via ActiveSync.* See the [blog post](https://grimhacker.com/2017/07/24/office365-activesync-username-enumeration/) and the [office365userenum](https://bitbucket.org/grimhacker/office365userenum/src/master/) tool.<br>
**byt3bl33d3r** - [SprayingToolkit](https://github.com/byt3bl33d3r/SprayingToolkit/)<br>
**sensepost** - [Ruler](https://github.com/sensepost/ruler/)