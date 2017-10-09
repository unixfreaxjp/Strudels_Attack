## Strudels Attack - Update information (2017, October)

Seeing the new pattern of [STRUDELS ATTACK](http://blog.malwaremustdie.org/2017/02/mmd-0062-2017-ssh-direct-tcp-forward-attack.html) to check the port range in the preliminary connecting stage before launching the attack:

[![](https://lh3.googleusercontent.com/pgsMWhnmBJpeTVQIByBGZJvWJ1HQsK0Dawfxrqu3kZLNT9BGt5TLyzSXJbazi7oGcr_GKZSolnJ-Zq7HLXABluwajGg_quG1kcSkoT-aLzT4Gk9azsFpedZOqdVgxjtwG-QdrWC54WI=w700-h599-no)](https://lh3.googleusercontent.com/pgsMWhnmBJpeTVQIByBGZJvWJ1HQsK0Dawfxrqu3kZLNT9BGt5TLyzSXJbazi7oGcr_GKZSolnJ-Zq7HLXABluwajGg_quG1kcSkoT-aLzT4Gk9azsFpedZOqdVgxjtwG-QdrWC54WI=w1463-h599-no)

It utilizes the **good site** of `portquiz.net` for checking the range of available internet global access, with the usage of java SSH library from **good tool** `jcraft.com` with the SSH version tag of: `SSH-2.0-JSCH-0.1.54` -

```asm
Remote SSH version: SSH-2.0-JSCH-0.1.54
```

-during the first connection. The upstream IP used are mostly coming from west Europe. 

The request for the port range checking can be analyzed as per below illustration:

[![](https://lh3.googleusercontent.com/aI8rxA5xKJQrhQ--SCBq9eR4enqtyl-mp7--JMGfsLLzOMOMujPstdAC86UmKZJ_-bHt2XhVvj9YGn1eD_VUtQa_A5RWXI8erzMcMNJHcEdrEFykRQ-jwfVy8_a2GieF0yDsY7l8jNQ=w700-h562-no)](https://lh3.googleusercontent.com/aI8rxA5xKJQrhQ--SCBq9eR4enqtyl-mp7--JMGfsLLzOMOMujPstdAC86UmKZJ_-bHt2XhVvj9YGn1eD_VUtQa_A5RWXI8erzMcMNJHcEdrEFykRQ-jwfVy8_a2GieF0yDsY7l8jNQ=w1723-h562-no)

The SSH compromising method is by the `login brute-force` attack, with the squence as per below,

```asm
2017-10-09 07:40:44 [sid/ip=926,195.154.51.223] login attempt [a/a] failed
2017-10-09 07:40:46 [sid/ip=926,195.154.51.223] login attempt [a/] failed
2017-10-09 07:40:47 [sid/ip=926,195.154.51.223] login attempt [a/a] failed
2017-10-09 07:40:54 [sid/ip=927,195.154.51.223] login attempt [adm/adm] failed
2017-10-09 07:40:56 [sid/ip=927,195.154.51.223] login attempt [adm/] failed
2017-10-09 07:40:58 [sid/ip=927,195.154.51.223] login attempt [adm/adm] failed
2017-10-09 07:41:05 [sid/ip=928,195.154.51.223] login attempt [admin/admin] failed
  :                                                              :
  
2017-10-08 23:27:30 [sid/ip=130,195.154.39.188] login attempt [a/a] failed
2017-10-08 23:27:32 [sid/ip=130,195.154.39.188] login attempt [a/] failed
2017-10-08 23:27:33 [sid/ip=130,195.154.39.188] login attempt [a/a] failed
2017-10-08 23:27:40 [sid/ip=131,195.154.39.188] login attempt [adm/adm] failed
2017-10-08 23:27:43 [sid/ip=131,195.154.39.188] login attempt [adm/] failed
2017-10-08 23:27:44 [sid/ip=131,195.154.39.188] login attempt [adm/adm] failed
2017-10-08 23:27:49 [sid/ip=132,195.154.39.188] login attempt [admin/admin] failed
  :                                                              :
```
-it will continue the brute until the SSH access is gained, and the attackers is using `diffie-hellman-group-exchange-sha1 ssh-rsa` as SSH auth scheme (supporting to the fact of actual tool used too), and `keyboard-interactive` authenticated access was used before gaining shell to show its human-interraction possibility.

```asm

2017-10-09 07:41:06 [sid/ip=928,195.154.51.223] xxx authenticated with keyboard-interactive
2017-10-08 23:27:49 [sid/ip=132,195.154.39.188] xxx authenticated with keyboard-interactive
```
We can see that both ip of `195.154.51.223` and `195.154.39.188` were accessing the compromised SSH with the same method..

Another trick that was used by attacker is the **keepaliving** stage to keep SSH channel's connection open to be used to launch the attack afterward. 
The monitored keepalive functions as per below, see the beacon range of `3seconds' and `4seconds` used:

```asm
2017-10-09 10:33:29 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:33 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:37 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:40 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:44 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:48 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:52 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:55 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:33:59 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:34:03 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:34:07 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:34:10 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:34:14 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:34:17 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
2017-10-09 10:34:21 [sid/ip=928,195.154.51.223] got global keepalive@jcraft.com request
  :                          :   :                                 :

2017-10-09 00:20:50 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:20:53 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:20:56 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:00 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:03 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:06 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:10 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:13 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:16 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:19 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:23 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:26 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:29 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:33 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
2017-10-09 00:21:36 [sid/ip=132,195.154.39.188] got global keepalive@jcraft.com request
  :                          :   :                                 :
```

So far I spotted these two attacker's IP source that is worth to be blocked from your infrastructure to mitigate these attack to your weak SSH services, like obsolete version of IoT or the weak login IoT devices, GeoIP is as follows:

```lua
{
  "ip": "195.154.51.223",
  "hostname": "195-154-51-223.rev.poneytelecom.eu",
  "city": "",
  "region": "",
  "country": "FR",
  "loc": "48.8582,2.3387",
  "org": "AS12876 ONLINE S.A.S."

  "ip": "195.154.39.188",
  "hostname": "195-154-39-188.rev.poneytelecom.eu",
  "city": "",
  "region": "",
  "country": "FR",
  "loc": "48.8582,2.3387",
  "org": "AS12876 ONLINE S.A.S."
}
```

Below is the attacker's BGP info, please notice the ASN & prefix used:

```asm
195.154.39.188 | 195-154-39-188.rev.poneytelecom.eu. | AS12876 | 195.154.0.0/16 | AS12876, | FR | FR
195.154.51.223 | 195-154-51-223.rev.poneytelecom.eu. | AS12876 | 195.154.0.0/16 | AS12876, | FR | FR
```

Don't hack other people's environment, it is bad and illegal, and you are tresspassing many law and rules.

I hope this post can help to stop the threat and its mitigation.

#MalwareMustDie
