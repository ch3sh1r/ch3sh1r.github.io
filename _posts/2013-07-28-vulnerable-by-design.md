---
layout: post
title: "Vulnerable by design"
abstract: "Список пазлов для тренировки."
tags: [ctf]
---
http://www.pentestit.ru/en/

Pentest lab. "Hacker" training. Deliberately insecure applications 
challenge thingys.
Call it what you will, but what happens when you want to try 
out your new set of skills? Do you want to be compare results 
from a tool when its used in different environments? What if 
you want to explore a system (that is legal to do so!) that you 
have no knowledge about (because you didnt set it up!)...

If any of that sounds helpful, below is a small collection of 
different environments, so if you want to go from "boot to root
", "capture the flag" or just to dig around as much as you want 
to try out the odd thing here and there. These will allow you 
to do so and without getting in trouble for doing it!

Все что возможно снабжено прохождением, но помни -- идея не
пройти все что есть а научиться чему-то новому.

Я знаю, что тысячи их. Но если ты хочешь порекомендовать какую-то --
сделай это!

* __ОС с ошибками в уязвимостями безопасности__
* __Локальные веб-сайты с уязвимостями в области безопасности__
* __Интернет веб-сайты с уязвимостями в области безопасности__
* __VPN варгейм__
* __Web варгейм__
* __Форензика__
* __Мобильные платформы__
* __Соревнования Capture The Flag__


## ОС с ошибками в уязвимостями безопасности
The idea of going from boot to root 
via any which way you can. Most of them have multiple entry points 
(some are easier than others) so you can keep using it ;)  They 
are all Linux OS (either in ISO or VM form) with vulnerable/configured 
software installed. (If you havent got any VM software, VMware 
Player is free and will do the trick)

##### [Damn Vulnerable Linux](http://www.damnvulnerablelinux.org)

__Описание:__ Damn Vulnerable Linux (DVL) is everything a good 
Linux distribution isn’t. Its developers have spent hours stuffing 
it with broken, ill-configured, outdated, and exploitable software 
that makes it vulnerable to attacks. DVL isn’t built to run on 
your desktop – it’s a learning tool for security students.

__Прохождение:__ Brochure


##### [De-ICE](http://www.de-ice.net)

__Описание:__ The PenTest LiveCDs are the creation of Thomas Wilhelm, 
who was transferred to a penetration test team at the company 
he worked for. Needing to learn as much about penetration testing 
as quickly as possible, Thomas began looking for both tools and 
targets. He found a number of tools, but no usable targets to 
practice against. Eventually, in an attempt to narrow the learning 
gap, Thomas created PenTest scenarios using LiveCDs.

Version/Levels: Level 1 - Disk 1, Level 1 - Disk 2, Level 1 - Disk 3 (A & B) Level 2 - Disk 1
Support/Walk-through: Forums, Wiki,  Level 1 - Disk 1, Level 1 - Disk 2, Level 1 - Disk 3, Level 2 - Disk 1


##### [Hackademic](http://ghostinthelab.wordpress.com/)

__Описание:__ Download the target and get root.After all, try to 
read the contents of the file “key.txt” in the root directory.

Version/Levels: 2 (Box 1, Box 2)
Support/Walk-through: N/A


##### [Holynix](http://pynstrom.net/holynix.php)

__Описание:__ Holynix is a Linux distribution that was deliberately 
built to have security holes for the purposes of penetration testing.

Version/Levels: 2
Support/Walk-through: Forum, SourceForge


##### [Kioptrix](http://www.kioptrix.com)

__Описание:__ This Kioptrix VM Image are easy challenges. The object 
of the game is to acquire root access via any means possible 
(except actually hacking the VM server or player).
The purpose of these games are to learn the basic tools and techniques 
in vulnerability assessment and exploitation. There are more 
ways then one to successfully complete the challenges.

Version/Levels: 3
Support/Walk-through: Blog, Level 1 - mod\_ssl, Level 2 - Injection, Level 3


##### [Metasploitable](http://blog.metasploit.com/2010/05/introducing-metasploitable.html)

__Описание:__ One of the questions that we often hear is "What 
systems can i use to test against?" Based on this, we thought 
it would be a good idea throw together an exploitable VM that 
you can use for testing purposes.

Version/Levels: 1
Support/Walk-through: Blog, DistCC, MySQL, PostgreSQL, TikiWiki, TomCat 


##### [NcN 2011](http://noconname.org)

__Описание:__ This machine has several users, one for each level
, so that exploiting the various challenges pose the participant 
will be changing and increasing user privileges. 

Version/Levels: 6 levels
Support/Walk-through: Download (Mirror), Rules


##### [NETinVM](http://informatica.uv.es/~carlos/docencia/netinvm/#id7)

__Описание:__ NETinVM is a single VMware virtual machine image 
that contains, ready to run, a series of User-mode Linux (UML
) virtual machines which, when started, conform a whole computer 
network inside the VMware virtual machine. Hence the name NETinVM
, an acronym for NETwork in Virtual Machine. NETinVM has been 
conceived mainly as an educational tool for teaching and learning 
about operating systems, computer networks and system and network 
security, but other uses are certainly possible.

Version/Levels: 3 (2010-12-01)
Support/Walk-through: Blog


##### [pWnOS](http://forums.heorot.net/viewtopic.php?f=21&t=149)

__Описание:__ Its a linux virtual machine intentionally configured 
with exploitable services to provide you with a path to r00t. 
Currently, the virtual machine NIC is configured in bridged 
networking, so it will obtain a normal IP address on the network 
you are connected to. You can easily change this to NAT or Host 
Only if you desire. A quick ping sweep will show the IP address 
of the virtual machine.

Version/Levels: 1
Support/Walk-through: Forums, Level 1



## (Offline) Web Based
Most of them youll need to download, copy 
and load the files yourself on your own web server (if you haven
t already got one, xampp is great). A few of them are VM images 
that can be loaded in to Virtual machines as they come with all 
the software & settings needed.


##### [BadStore](http://www.badstore.net/)

__Описание:__ Badstore.net is dedicated to helping you understand how hackers prey on Web application vulnerabilities, and to showing you how to reduce your exposure. Our Badstore demonstration software is designed to show you common hacking techniques.

Version/Levels: 1 (v1.2)
Support/Walk-through: PDF


##### [BodgeIT](https://code.google.com/p/bodgeit/)

__Описание:__ The BodgeIt Store is a vulnerable web application which is currently aimed at people who are new to pen testing.

Version/Levels: 1 (v1.3.0)


##### [Damn Vulnerable Web App](http://www.dvwa.co.uk/)

__Описание:__ Damn Vulnerable Web App (DVWA) is a PHP/MySQL web application that is damn vulnerable. Its main goals are to be an aid for security professionals to test their skills and tools in a legal environment, help web developers better understand the processes of securing web applications and aid teachers/students to teach/learn web application security in a class room environment.

Version/Levels: 1 (v1.0.7)
Support/Walk-through: PDF


##### [HackUS HackFest Web CTF](http://hackus.org/en/media/training/ & http://www.h3xstream.com/codeView.jspx?key=4001)

__Описание:__ The Hackfest is an annual event held in Quebec city. For each event, a competition is held where participants competed at solving challenges related to security. For the 2010 edition, I got involved in the competition by creating the web portion of the competition.

Version/Levels: 1 (2010)
Support/Walk-through: Blog, Solutionnaire (English)


##### [Hacme](http://www.mcafee.com/us/downloads/free-tools/index.aspx)

__Описание:__ Foundstone Hacme Casino™ is a learning platform for secure software development and is targeted at software developers, application penetration testers, software architects, and anyone with an interest in application security.

Version/Levels: 5 (2006)
Support/Walk-through: Bank, Book, Casino, Shipping, Travel


##### [Hackxor](http://hackxor.sourceforge.net/cgi-bin/index.pl)

__Описание:__ Hackxor is a webapp hacking game where players must locate and exploit vulnerabilities to progress through the story. Think WebGoat but with a plot and a focus on realism&difficulty. Contains XSS, CSRF, SQLi, ReDoS, DOR, command injection, etc

Version/Levels: 1
Support/Walk-through: Online Version, Cryptic spoiler-free hints


##### [LAMPSecurity](http://sourceforge.net/projects/lampsecurity/)

__Описание:__ Foundstone Hacme Casino™ is a learning platform for secure software development and is targeted at software developers, application penetration testers, software architects, and anyone with an interest in application security.

Version/Levels: v6 (4x)
Support/Walk-through: SourceForge


##### [Moth](http://www.bonsai-sec.com/en/research/moth.php)

__Описание:__ Moth is a VMware image with a set of vulnerable Web Applications and scripts.

Version/Levels: v6 
Support/Walk-through: SourceForge


##### [Mutillidae](http://www.irongeek.com/i.php?page=security/mutillidae-deliberately-vulnerable-php-owasp-top-10)

__Описание:__ Mutillidae: A Deliberately Vulnerable Set Of PHP Scripts That Implement The OWASP Top 10

Version/Levels: v1.5
Support/Walk-through: N/A


##### [OWASP Broken Web Applications Project](https://code.google.com/p/owaspbwa/ or https://www.owasp.org/index.php/OWASP_Broken_Web_Applications_Project)

__Описание:__ This project includes applications from various sources 
(listed in no particular order).

Intentionally Vulnerable Applications:
* OWASP WebGoat version 5.3.x(Java)
* OWASP Vicnum version 1.4 (PHP/Perl)
* Mutillidae version 1.5 (PHP)
* Damn Vulnerable Web Application version 1.07.x (PHP)
* Ghost (PHP)
* Peruggia version 1.2 (PHP)
* OWASP CSRFGuard Test Application version 2.2 (Java)
* OWASP AppSensor Demo Application (Java)
* Mandiant Struts Forms (Java/Struts)
* Simple ASP.NET Forms (ASP.NET/C#)
* Simple Form with DOM Cross Site Scripting (HTML/JavaScript)

Old Versions of Real Applications:
* WordPress 2.0.0 (PHP, released December 31, 2005, downloaded from www.oldapps.com)
* phpBB 2.0.0 (PHP, released April 4, 2002, downloaded from www.oldapps.com)
* Yazd version 1.0 (Java, released February 20, 2002)
* gtd-php version 0.7 (PHP, released September 30, 2006)
* OrangeHRM version 2.4.2 (PHP, released May 7, 2009)
* GetBoo version 1.04 (PHP, released April 7, 2008)

Version/Levels: v0.92rc1
Support/Walk-through: N/A


##### [OWASP Hackademic Challenges Project](https://www.owasp.org/index.php/OWASP_Hackademic_Challenges_Project)

__Описание:__ The OWASP Hackademic Challenges Project is an open source project that helps you test your knowledge on web application security. You can use it to actually attack web applications in a realistic but also controlable and safe environment. 

Version/Levels: 1 (Live Version)
Support/Walk-through: GoogleCode (Download Offline Version)


##### [OWASP Insecure Web App Project](https://www.owasp.org/index.php/Category:OWASP_Insecure_Web_App_Project)

__Описание:__ InsecureWebApp is a web application that includes common web application vulnerabilities. It is a target for automated and manual penetration testing, source code analysis, vulnerability assessments and threat modeling.

Version/Levels: 1
Support/Walk-through: N/A


##### [OWASP Vicnum](http://vicnum.ciphertechs.com/)

__Описание:__ A mirror of deliberately insecure applications and old softwares with known vulnerabilities. Used for proof-of-concept /security training/learning purposes. Available in either virtual images or live iso or standalone formats 

Version/Levels: 1.4 (2009)
Support/Walk-through: SourceForge (Download Offline Version)


##### [OWASP WebGoat](http://www.owasp.org/index.php/Category:OWASP_WebGoat_Project)

__Описание:__ WebGoat is a deliberately insecure J2EE web application maintained by OWASP designed to teach web application security lessons. In each lesson, users must demonstrate their understanding of a security issue by exploiting a real vulnerability in the WebGoat application.

Version/Levels: 1
Support/Walk-through: User Guide, GoogleCode, SourceForge


##### [PuzzleMall](https://code.google.com/p/puzzlemall/)

__Описание:__ PuzzleMall is a vulnerable web application designed for training purposes.It is prone to a variety of different session puzzle exposures, which can be detected and exploited using different session puzzling sequences.

Version/Levels: 1
Support/Walk-through: N/A


##### [SecuriBench](http://suif.stanford.edu/~livshits/securibench/)

__Описание:__ Stanford SecuriBench is a set of open source real-life programs to be used as a testing ground for static and dynamic security tools. Release .91a focuses on Web-based applications written in Java

Version/Levels: Normal, Micro
Support/Walk-through: N/A


##### [The ButterFly](http://sourceforge.net/projects/thebutterflytmp/)

__Описание:__ The ButterFly project is an educational environment intended to give aninsight into common web application and PHP vulnerabilities. The environment alsoincludes examples demonstrating how such vulnerabilities are mitigated.

Version/Levels: 1
Support/Walk-through: N/A


##### [UltimateLAMP](http://ronaldbradford.com/blog/ultimatelamp-2006-05-19/)

__Описание:__ UltimateLAMP is a fully functional environment allowing you to easily try and evaluate a number of LAMP stack software products without requiring any specific setup or configuration of these products. UltimateLAMP runs as a Virtual Machine with VMware Player (FREE). This demonstration package also enables the recording of all user entered information for later reference, indeed you will find a wealth of information already available within a number of the Product Recommendations starting with the supplied Documentation.

Version/Levels: v0.2
Support/Walk-through: Passwords


##### [Virtual Hacking Lab](http://virtualhacking.sourceforge.net/)

__Описание:__ A mirror of deliberately insecure applications and old softwares with known vulnerabilities. Used for proof-of-concept /security training/learning purposes. Available in either virtual images or live iso or standalone formats

Version/Levels: 1
Support/Walk-through: SourceForge


##### [WackoPicko](https://github.com/adamdoupe/WackoPicko)

__Описание:__ WackoPicko is a vulnerable web application used to test web application vulnerability scanners.

Version/Levels: 1
Support/Walk-through: N/A


##### [WAVSEP - Web Application Vulnerability Scanner Evaluation Project](https://code.google.com/p/wavsep/)

__Описание:__ A vulnerable web application designed to help assessing the features, quality and accuracy of web application vulnerability scanners.This evaluation platform contains a collection of unique vulnerable web pages that can be used to test the various properties of web application scanners.

Version/Levels: 1
Support/Walk-through: N/A


##### [WebMaven/Buggy Bank](http://www.mavensecurity.com/WebMaven/)

__Описание:__ WebMaven (better known as Buggy Bank) was an interactive learning environment for web application security. It emulated various security flaws for the user to find. This enabled users to safely & legally practice web application vulnerability assessment techniques. In addition, users could benchmark their security audit tools to ensure they perform as advertised.

Version/Levels: v1.0.1
Support/Walk-through: Download


##### [Web Security Dojo](http://www.mavensecurity.com/web_security_dojo/)

__Описание:__ A free open-source self-contained training environment for Web Application Security penetration testing. Tools + Targets = Dojo

Various web application security testing tools and vulnerable web applications were added to a clean install of Ubuntu v10.04.1, which is patched with the appropriate updates and VM additions for easy use.
Version 1.1 includes an exclusive speed-enhanced version of Burp Suite Free. Special thanks to PortSwigger .
Version/Levels: 1
Support/Walk-through: SourceForge


## (Online) Web Based 
Same as above, however if you dont want 
the hassle of setting it all up or to be able to do it where 
ever you have a Internet connection...

##### [Biscuit](http://heideri.ch/biscuit/)

__Описание:__ Goal: alert(document.cookie) // extract the PHPSESSID, FF3.6 - 4 only!

Version/Levels: 1
Support/Walk-through: N/A


##### [Gruyere / Jarlsberg](http://google-gruyere.appspot.com/)

__Описание:__ This codelab shows how web application vulnerabilities can be exploited and how to defend against these attacks. The best way to learn things is by doing, so youll get a chance to do some real penetration testing, actually exploiting a real application

Version/Levels: 1 (v1.0.7)
Support/Walk-through: PDF Download offline


##### [HackThis](http://www.hackthis.co.uk/)

__Описание:__ Welcome to HackThis!!, this site was set up over 2 years ago as a safe place for internet users to learn the art of hacking in a controlled environment, teaching the most common flaws in internet security.

Version/Levels: 32 (40?)
Support/Walk-through: N/A


##### [hACME](http://www.hacmegame.org/hacmegame/main/welcome.html)

__Описание:__ hACME game is software security learning game, mainly concerning web applications. The game is intended to help raise awareness and interest in the subject of software security as well as train developers. The purpose of the game is not to train hackers, but to make future software developers aware of how important security is.

Version/Levels: Lots
Support/Walk-through: N/A


##### [Hackxor](http://hackxor.sourceforge.net/cgi-bin/index.pl)

__Описание:__ Hackxor is a webapp hacking game where players must locate and exploit vulnerabilities to progress through the story. Think WebGoat but with a plot and a focus on realism&difficulty. Contains XSS, CSRF, SQLi, ReDoS, DOR, command injection, etc

Version/Levels: 1
Support/Walk-through: Online Version, cryptic spoiler-free hints


##### [OWASP Hackademic Challenges Project](https://www.owasp.org/index.php/OWASP_Hackademic_Challenges_Project)

__Описание:__ The OWASP Hackademic Challenges Project is an open source project that helps you test your knowledge on web application security. You can use it to actually attack web applications in a realistic but also controlable and safe environment. 

Version/Levels: 1 (Live Version)
Support/Walk-through: GoogleCode (Download Offline Version)


##### [OWASP Vicnum](http://vicnum.ciphertechs.com/)

__Описание:__ A mirror of deliberately insecure applications and old softwares with known vulnerabilities. Used for proof-of-concept /security training/learning purposes. Available in either virtual images or live iso or standalone formats 

Version/Levels: 1.4 (2009)
Support/Walk-through: SourceForge (Download Offline Version)


##### [PCTechTips - pwn3d the login form.](http://pctechtips.org/hacker-challenge-pwn3d-the-login-form/)

__Описание:__  I came up with this pwn3d zit3 login form challenge, to kind of expose one of the many web application vulnerabilities; it consists of a login form which authenticates against a mysql backend database to give authorized access to the members only part of the web site (you must become a member first—>”REGISTER”)

Version/Levels: 1
Support/Walk-through: N/A


##### [XSSMe](http://xssme.html5sec.org/)

__Описание:__ Find a way to steal document.cookie w/o user interaction

Version/Levels: 1
Support/Walk-through: N/A


##### [XSS Me!](http://html5sec.org/xssme.php)

__Описание:__ XSS ME! (vulnerable param: GET[xss])

Version/Levels: 1
Support/Walk-through: N/A


##### [Can You XSS This?](http://canyouxssthis.com/HTMLSanitizer/)

__Описание:__ XSS ME! (vulnerable param: GET[xss])

Version/Levels: 1
Support/Walk-through: N/A


##### [Test x5s](http://www.nottrusted.com/x5s/)

__Описание:__ This will give you a small working example of how to use x5s to find encoding and transformation issues that can lead to XSS vulnerability.

Version/Levels: 1
Support/Walk-through: N/A


##### [XSS Progphp](http://xss.progphp.com/)

__Описание:__ This site has a number of XSS problems. See if you can find them all.

Version/Levels: 1
Support/Walk-through: N/A


##### [XSS Quiz](http://xss-quiz.int21h.jp)

__Описание:__ XSS it.

Version/Levels: Lots
Support/Walk-through: N/A


## WarGames (VPN)
These are computer break-in challenge, were 
you try and compete against other users. They are usually ranked
 in which you collect points, over multiple levels to make your 
way onto a Hall Of Fame (Top 10,25,50 or 100 Users). This all 
takes part on a separate private network, where you have to connect 
into it each time.

##### [Hacking-Lab](http://www.hacking-lab.com/)

__Описание:__ This ist the LiveCD project of Hacking-Lab (www.hacking-lab.com). It gives you OpenVPN access into Hacking-Labs Remote Security Lab. The LiveCD iso image runs very good natively on a host OS, or within a virtual environment (VMware, VirtualBox).

The LiveCD gives you OpenVPN access into Hacking-Lab Remote.You will gain VPN access if both of the two pre-requirements are fulfilled.
Version/Levels: 1 (v5.30)
Support/Walk-through: Download


##### [OverTheWire](http://www.overthewire.org/wargames/)

__Описание:__ The wargames offered by the OverTheWire community can help you to learn and practice security concepts in the form of funfilled games. 

Levels: 7
Support/Walk-through: N/A


##### [pwn0](https://pwn0.com/home.php)

__Описание:__ Just sign up, connect to the VPN, and start hacking.

Levels:1
Support/Walk-through: N/A


## WarGames (Web Based)


##### [HackThisSite](http://www.hackthissite.org/)

__Описание:__ Hack This Site is a free, safe and legal training ground for hackers to test and expand their hacking skills. More than just another hacker wargames site, we are a living, breathing community with many active projects in development, with a vast selection of hacking articles and a huge forum where users can discuss hacking, network security, and just about everything. Tune in to the hacker underground and get involved with the project.

Version/Levels: Lots
Support/Walk-through: N/A


##### [Enigma Group - Training Missions](http://www.enigmagroup.org/pages/basics/)

__Описание:__ Have you ever wanted to learn how to hack? Are you more of a hands on learner, then one that can learn from just reading out of a book? Are you interested in developing secure code by understanding how a hacker will attack your application? If you answered "yes" to any of these questions, then this site is for you.

Version/Levels: Lots
Support/Walk-through: N/A


##### [HellBoundHackers](http://www.hellboundhackers.org/)

__Описание:__ The hands-on approach to computer security.Learn how hackers break in, and how to keep them out.

Levels: Lots
Support/Walk-through: N/A


##### [SmashTheStack](http://www.smashthestack.org)

__Описание:__ The Smash the Stack Wargaming Network hosts several Wargames. A Wargame in our context can be described as an ethical hacking environment that supports the simulation of real world software vulnerability theories or concepts and allows for the legal execution of exploitation techniques. Software can be an Operating System, network protocol, or any userland application.

Levels: Lots
Support/Walk-through: N/A


##### [Wechall](https://www.wechall.net)

__Описание:__ For the people not familiar with challenge sites, a challenge site is mainly a site focussed on offering computer-related problems. Users can register at such a site and start solving challenges. There exist lots of different challenge types. The most common ones are the following: Cryptographic, Crackit, Steganography, Programming, Logic and Math/Science. The difficulty of these challenges vary as well.

Version/Levels: Lots
Support/Walk-through: N/A


##### [VulnerabilityAssessment](http://www.vulnerabilityassessment.co.uk)

__Описание:__ Hopefully a valuable information source for Vulnerability Analysts and Penetration Testers alike.

Version/Levels: Lots
Support/Walk-through: N/A


##### [Net-Force](http://net-force.nl)

__Описание:__ N/A

Version/Levels: Lots
Support/Walk-through: N/A


##### [Hack Quest](http://hackquest.com)

__Описание:__ This site offers a unique hack challenge especially for beginners and intermediates.

Version/Levels: Lots
Support/Walk-through: N/A


## Forensic
The idea is to analysis event(s) to see if you can 
understand what either has been going or or happening currently
. Some are complete disk images with scenarios, whereas some 
are single exercises (e.g. Data Carving, Memory Dump Analysis
 or Reserve Engineering).

##### [Binary-Auditing](http://www.binary-auditing.com/)

__Описание:__ Learn the fundamentals of Binary Auditing. Know how HLL mapping works, get more inner file understanding than ever.

Try to solve brain teasing puzzles with our collection of copy protection games. Increasing difficulty and unseen strange tricks.
Learn how to find and analyse software vulnerability. Dig inside Buffer Overflows and learn how exploits can be prevented.
Start to analyse your first viruses and malware the safe way. Learn about simple tricks and how viruses look like using real life examples.
Version/Levels: Lots
Support/Walk-through: N/A


##### [Digital Forensics Tool Testing Images](http://dftt.sourceforge.net/)

__Описание:__ To fill the gap between extensive tests from NIST and no public tests, I have been developing small test cases. The following are file system and disk images for testing digital (computer) forensic analysis and acquisition tools.

Version/Levels: 14
Support/Walk-through: N/A


##### [Digital Corpora - DiskImages & Scenarios](http://digitalcorpora.org/corpora/disk-images & http://digitalcorpora.org/corpora/scenarios)

__Описание:__ We have many sources of disk images available for use in education and research. The easiest disk images to work with are the NPS Test Disk Images.   

Scenarios are collections of multiple disk images, memory dumps, network traffic, and/or data from portable devices.
Version/Levels: 3 + 7
Support/Walk-through: N/A


##### [DFRWS 2011 Forensics Challenge](http://www.dfrws.org/2011/challenge/)

__Описание:__ Given the variety and impending ubiquity of Android devices along with the wide range of crimes that can involve these systems as a source of evidence, the DFRWS has created two scenarios for the forensics challenge in 2011.

Version/Levels: 2
Support/Walk-through: N/A


##### [ForensicKB](http://www.forensickb.com/search/label/Forensic%20Practical)

__Описание:__ We have many sources of disk images available for use in education and research. The easiest disk images to work with are the NPS Test Disk Images.

Version/Levels: Level 1, Level 2, Level 3, Level 4
Support/Walk-through: Level 1 - Solution


##### [Honeynet Project Challenges](https://www.honeynet.org/challenges)

__Описание:__ The purpose of Honeynet Challenges is to take this learning one step farther. Instead of having the Honeynet Project analyze attacks and share their findings, Challenges give the security community the opportunity to analyze these attacks and share their findings. The end results is not only do individuals and organizations learn about threats, but how to learn and analyze them. Even better, individuals can see the write-ups from other individuals, learning new tools and technique for analyzing attacks. Best of all, these attacks are from the wild, real hacks.

Version/Levels: 8
Support/Walk-through: N/A


##### [SecuraLabs Challenge](http://www.securabit.com/)

__Описание:__ Part 1 - What is the name of exploit kit being used in this pcap (not the verison, you may include the entire string on that line)?

Part 2 - the decryption key will be the main name of the exploit kit all in lower case without spaces, and without the version or anything else on that line in the file.
Part 3 - Submit a working key and serial.
Version/Levels: Two (One, Two)
Support/Walk-through: N/A


## Mobile Platforms
The same idea the subjects above, however these 
programs are designed for mobile use on smart phones. The increase 
of mobile phone usage is on the rise, along with smart phones
. As more and more programs are being created for this platform
, this makes the possibly of more poorly coded programs and/or
higher change of malware. These programs are meant to defend 
against these program defects.

##### [ExploitMe](http://labs.securitycompass.com/tools/new-mobile-security-course-and-exploitme-mobile/)

__Описание:__ If your organization is working with mobile applications this course is a fantastic primer on how mobile apps can be hacked, and how your teams can defend against these software defects.

Version/Levels: One
Support/Walk-through: Android, iPhone


## Capture The Flag Competitions
At various events, competitions 
are run to attack and defend computers and networks. Here is 
a list of resources which were used.

##### [CSAW (Cyber Security Awareness Week) CTF](http://www.poly.edu/csaw2011)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: N/A


##### [CodeGate 2011](http://www.codegate.org/Eng/)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: Write up


##### [Defcon 19](https://www.defcon.org)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: N/A


##### [Hacklu](http://2011.hack.lu/index.php/Main_Page)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: Write Up


##### [ISEC CTF WarFare](http://isec2011.wowhacker.com)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: N/A


##### [Plaid CTF](http://www.plaidctf.com)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: Write Ups


##### [RSSIL](http://www.rssil.org)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: Write Up


##### [Insomnihack 2k11](https://blog.fortinet.com/insomnihack-2011/)

__Описание:__ N/A

Version/Levels: 2011
Support/Walk-through: N/A


##### [RuCTFE 2010](http://ructf.org/e/2010/)

__Описание:__ RuCTFE is a remote challenge in information security

Version/Levels: 1
Support/Walk-through: Network Setup


## Other collections & lists

Practice Labs at Hacking Cisco - 
http://packetlife.net/blog/2011/apr/15/practice-labs-hacking-cisco/

Vulnerable Web Applications for learning - 
https://securitythoughts.wordpress.com/2010/03/22/vulnerable-web-applications-for-learning/

Pentesting Vulnerable Study Frameworks Complete List - 
http://www.felipemartins.info/2011/05/pentesting-vulnerable-study-frameworks-complete-list/

RSnakes Vulnerability Lab - 
http://ha.ckers.org/weird/ 

Pentest lab vulnerable servers-applications list - 
http://bailey.st/blog/2010/09/14/pentest-lab-vulnerable-servers-applications-list/
