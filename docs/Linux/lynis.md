# lynis - Security Auditing Tool

> Lynis - Security auditing tool for Linux, macOS, and UNIX-based systems.<br />
> Assists with compliance testing (HIPAA/ISO27001/PCI DSS) and system hardening.<br />
> Agentless, and installation optional.
>

Home Page: <https://cisofy.com/lynis/>

Source : <https://github.com/CISOfy/lynis>

Video Reference: <https://www.youtube.com/watch?v=5tDEYkWD338><br />
Video is about [Alpine Linux](./Distro/alpine.md "Alpine Linux Installation help")
but in the End they run `lynis` to find about security.

This tool can help to secure *Linux systems* and evaluate its threat counter measures.

## Installation

Typically this tool is executed by *System Administrators* to check
the security capability of the system.

There are two Steps:

1. Clone or Download the *[Git Repo](https://github.com/CISOfy/lynis)*<br />
      ```sh
       git clone https://github.com/CISOfy/lynis
      ```
2. Execute the Command :
      ```sh
      cd lynis
      sudo chown -R 0:0 .
      sudo ./lynis audit system
      ```
      <br />
      This command would analyze the system and provide with suggestions.

For better understanding refer to the **[Documentation](https://cisofy.com/documentation/lynis/)**

This can be run again after the better security is in place.
And, needs to be repeated at a periodic basis.

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Hub](./cli/README.md)
- [Back to Linux Hub](./README.md)
- [Back to Root Document](./README.md) Return to the Root
