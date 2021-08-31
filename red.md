# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:


  ```bash
      $ nmap 192.168.1.0/24
  ```

This scan identifies the services below as potential points of entry:
- Target 1
  - List of Several IP's that are online and each IP has several ports that   are left open.
  - Several have Port 22 (SSH), 80 (HTTP), 111 (RPCBIND), 445 (MICRO-DS).
  - These are all possible vulnerabilities that need to be monitored/hardened because attackers can use these ports that are open to find a possible entry to the system.
  - I checked to see what the HTTP servers had on them and found a Raven Security Page that had tons of information that allowed me to learn more about the system and found that it is a wordpress site, which is known for weak security and has several holes for attackers.

The following vulnerabilities were identified on each target:
- Target 1
  - Once I found out that there was a site I ran a common directory search using a python script called dirsearch

  ```bash
    $ python3 dirsearch.py -u 192.168.1.110
  ```
  - This didn't lead to anything substantial so I then ran a WPScan knowing its a WordPress site to find vulnerabilities and possible users:

  ```bash
  $ wpscan --url 192.168.1.110/wordpress --enumerate u
  ```
  - This gave me some substantial information and found 2 users that were on the system:

	`Steven & Michael`

  - This scan and enumeration technique gave me more than I needed to continue with the further attacks


### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: flag1{b9bbcb33e11b80be759c4e844862482d}
    - I found this flag later in the engagement, Once I had access to the system I did some digging around and was able to find it inside of the service.html file
      - I exploited Michael's weak password and used it to gain ssh access to the system
      - Once I had access I ran
	```bash
	cat /var/www/html/service.html | grep -i flag
	```

  - `flag2.txt`: flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
    - Weak Password
      - The User Michael had a weak password with it being Michael and I was able to ssh into the system
      - I then ran `locate flag.txt` to look for flags and found it

  - `flag3.txt` : flag3{afc01ab56b50591e7dccf93122770cd2}
      - I gained access to the mySQL db inside of the wp-config.php file which contained the login for the root user:

      ```bash
	DB_NAME = wordpress
	DB USER = root
	DB PASS = R@v3nSecurity
      ```
      - Once logged into the DB I looked at all the tables that were inside the DB and ran:

       ```bash
       SHOW TABLES;
       SELECT * FROM wp_users
       ```
      - This then gave me two hashes for the passwords of users Steven and Michael
    ![](images/hashes.PNG)

      - I found this flag inside of the mySQL database under the wp_posts

  - `flag4.txt` : flag4{715dea6c055b9fe3337544932f2941ce}
      - Once I got access to Steven's account via the mySQL DB weak hashes I looked for sudo privilege's and found that I had access to use Python
      - Once I figured out that Stevens allowed to use Python I looked online for possible ways to escalate privilege's using Python and found a good resource

        `https://gtfobins.github.io/gtfobins/python/`

      ```bash
      sudo python -c 'import os; os.system("/bin/sh")'
      ```

     - This gave me ROOT access and then was able to find the Root flag
