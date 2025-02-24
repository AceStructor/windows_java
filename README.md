# Ansible Role: windows_java

Ansible role to upgrade JRE/JDK to version 8 Update 221. Tasks in this role will do the following.

1. Log information about existing versions of Java (jre/jdk) under C:\JavaPreviousVersions.txt
2. Uninstall all existing versions of Java (jre/jdk) from the target machine
3. Ugrade to the latest version of Java (jre/jdk)
4. Upgrade will maintan bit versions which means if 32 bit version was previously present, the latest version being installed will also be 32bit and likewise for 64bit.
5. The above default feature can be overridden by tags that have provided.
6. Whichever component of Java (jre/jdk) was previously present on the machine will be upgraded.
7. Cleanup packages that were copied to the target machine.

Requirements
------------
This role assumes that there is a central repository where the downloaded packages of the latest versions are present. Below is the folder structure:

`\\your_repository_name\jdk\jdk8_221_64.exe` and `\\your_repository_name\jdk\jdk8_221_32.exe`

`\\your_repository_name\jre\jre-8u221-windows-x64.msi` and `\\your_repository_name\jre\jre-8u221-windows-i586.msi`

*Make sure to set the variable "repository_path" as the path to your repo, the role otherwise wouldn't work.*

Role Variables
--------------
*jdk/jre packages will be copied to target machines under the stage directory*
	
	---
	stage: C:\temp

*complete path of packages on the target machines after they have been copied*

	---
	_64_jdk_package_path: C:\temp\jdk\jdk8_221_64.exe
	_32_jdk_package_path: C:\temp\jdk\jdk8_221_32.exe
	_64_jre_package_path: C:\temp\jre\jre-8u221-windows-x64.msi
	_32_jre_package_path: C:\temp\jre\jre-8u221-windows-i586.msi

*used for creates_path. This allows installation of packages without knowing the product_id*

	---
	_64jdk: C:\Program Files\Java\jdk
	_64jre: C:\Program Files\Java\jre
	_32jdk: C:\Program Files (x86)\Java\jdk
	_32jre: C:\Program Files (x86)\Java\jre


Dependencies
------------
None

Example Playbook
----------------

*Uninstall all versions of Java and upgrade those versions to the latest*

        ---
        - hosts: servers
          roles:
            - role: rishihegde.windows_java
              vars:
                repository_path: \\127.0.0.1

*Use tags to only install specific versions or only uninstall*

        ---
        - hosts: servers
          roles:
            - role: rishihegde.windows_java
              tags: ['uninstall']
              vars:
                repository_path: \\127.0.0.1

        ---
        - hosts: servers
          roles:
            - role: rishihegde.windows_java
              tags: ['uninstall','jdk']
              vars:
                repository_path: \\127.0.0.1


*Available tags*
1. uninstall - only uninstalls all versions of jre/jdk that currently exists on the target machine
2. jdk - installs jdk in 32 Bit and 64 Bit if the System is 64 Bit
3. jre - installs jre in 32 Bit and 64 Bit if the System is 64 Bit

License
-------

BSD

Author Information
------------------
Rishi Hegde	rishihegde810@gmail.com
