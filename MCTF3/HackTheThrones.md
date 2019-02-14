![Alt text](./Pictures/MCTF3.jpg?raw=true "MCTF3-Logo")
# [MCTF3 Qualification round] [Hack The Throne (Web) — HackTheThrone2,3,4]

##### MCTF3 was an online CTF qualification round organized by th3j4ckers team. I played with Sudo_root team, and we finished #1.

------------
*Sorry for the bad writeup because of not taking screenshots of the challenges's descriptions and the needed information... *

------------



MCTF3 web challenges (HackTheThrones) were kind of a wargame "Solving the first challenge will give you access to the second one".
You can read the writeup of the first challenge  done by Hafidh Steg ( ͡° ͜ʖ ͡°) : [HackTheThrones1](https://medium.com/%400x000c0ded/mctf3-qualification-round-hack-the-throne-web-hackthethrone1-25pts-d8560fbee6e6)

**Challenge description** : There are some books that have not been indexed yet. (or something like that..)

So, accessing http://37.187.186.219:8080/hack_the_throne/dev_b3ta/ give us a page with some public and private books. We can only have access to the public books ( 6 books indexed from 0 to 5 ). 
After noticing thie id parameter when accessing a public book : http://37.187.186.219:8080/hack_the_throne/dev_b3ta/books.php?type=public&id=0
And since the challenge description was about indexing books, i decided to brute-force the id parameter.
I made a little bash script :

    #!/bin/bash
    for i in $(seq 1 10000);
    do
    		curl -s --url "http://37.187.186.219:8080/hack_the_throne/dev_b3ta/books.php?type=public&id=$i" |  wc -c;
    done
and here we got an interesting id :
![Alt text](./Pictures/brute-public.png?raw=true "public")
Checking the code source of :  http://37.187.186.219:8080/hack_the_throne/dev_b3ta/books.php?type=public&id=356, give us a comment with : (/4dm1n_p4n3l/)
The level2 was like :
![Alt text](./Pictures/tsema.png?raw=true "tsema")
I contacted my teammate Zaki about it, and we both thought it must be the 3rd level of HackTheThrones.
We had a login page at : http://37.187.186.219:8080/hack_the_throne/dev_b3ta/4dm1n_p4n3l/ which was vulnerable to an SQLi but first we should bypass the filter :
![Alt text](./Pictures/login.png?raw=true "tsema")
Zaki took care of it, bypassed the filter and we got two gifts :
1. Valid credentials : | jacktherippe | sU_p3r_s3c_r3t_pas5w0rd |
2. Flag of the 3rd level : MCTF3{jack_1s_th3_R3al_RipPeR_j0hn_i5_fake}

But since we didn't validate the 2nd challenge, we couldn't validate the 3rd one.
Zaki contacted the admin and it was so nice from him to open the challenge for us before submitting the 2nd one.


------------


Everyone in the team saw the cookie {"priveldge":"user"} that was base64encoded, but we didn't know what to use as name for root, we tried brute forcing but no clue... ='((
Then Yacine said that he started the challenge then he stopped and he found the name we should use...
it was 'superuser' -.-" How the fuck we didn't guess that !
After using the right cookie, i had access to the private book but there was nothing interesting...
Then again i decided to brute-force the id parameter for the private books and i got the id=197 that had the flag for the 2nd level : MCTF3{th3_b4st_B00ks_T0_r34d_are_th3_hard3sT_0nes_To_f1nd}


------------

it's a bit depressing : solving the next challenge then comeback to the one before then jump two steps... but yeah we solved them.
It's time to use the credentials we found before. now we have an upload page at : http://37.187.186.219:8080/hack_the_throne/dev_b3ta/4dm1n_p4n3l/admin.php?action=uploads
i prepared my shell and set up an ngrok tcp server to get an interactive shell.
![Alt text](./Pictures/ngrok.png?raw=true "ngrok")
After getting  a shell, we found a file (forgot the name... x"D) that had the flag :
MCTF3{whY_Th3_H3ll_th3r3_1s_an_UploaD_f0rm}


------------

The last level was about getting priveledge escalation and reading a file in the root directory, Red and i started to search for interesting file... we end up with find the file /etc/shadow with 777 permission 
![Alt text](./Pictures/shell.png?raw=true "shell")
since we had only 10mn left, we panicked a bit to have problems adding a new entry to the shadow file because there were others modifying the same file...
The ctf ended and we couldn't cat the file passwd file... =((


------------

##### I would like to thank the organisers ( J4CK3RS ) for the good challenges and the fair-play ;) 


