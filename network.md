# Network Forensic Analysis Report


## Time Thieves
You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users' custom site?

      wpad.frank-n-ted.com

2. What is the IP address of the Domain Controller (DC) of the AD network?

    10.6.12.12

3. What is the name of the malware downloaded to the 10.6.12.203 machine?

    `filter = ip.addr == 10.6.12.203`


4. What kind of malware is this classified as?

    - I found that the malware was a DLL file called:

        `june11.dll`

    - It is classified as a Trojan and is flagged by 49 vendors

 ![](https://github.com/garrettduardo123/Final-Project/blob/main/Resources/malware.PNG)

---

## Vulnerable Windows Machine

1. Find the following information about the infected Windows machine:
    - Host name:

    mind-hammer, LenevoEM_b0:63:a4

    - IP address:

     172.16.4.205

    - MAC address:

    00:59:07:b0:63:a4

2. What is the username of the Windows user whose computer is infected?

    AS-REQ
    
![](https://github.com/garrettduardo123/Final-Project/blob/main/Resources/usernameinfect.PNG)

3. What are the IP addresses used in the actual infection traffic?

    172.16.4.205

    172.16.4.4

---

## Illegal Downloads

1. Find the following information about the machine with IP address `10.0.0.201`:

    `filter = ip.addr == 10.0.0.201`

    - MAC address

        00:16:17:18:66:c8

    - Windows username

    Blanco-Desktop

    - OS version

    `filter = ip.addr == 10.0.0.201 and http`

    Windows NT 10.0

2. Which torrent file did the user download?

    `filter = ip.addr == 10.0.0.201 and http.request.uri contains ".torrent"`

    Betty_Boop_Rhythm_on_the_Reservation.avi.torrent
    
 ![](https://github.com/garrettduardo123/Final-Project/blob/main/Resources/torrent.PNG)
