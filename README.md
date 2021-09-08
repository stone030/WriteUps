# some of AMAN CTF writeups
This is my 1st time doing a writeup, so for any adjustment plz let me know :)
The writeups here are presented for education purpose only :)
for any comment you can either send it here or to Stone's social accounts directly, though i sometimes take days to reply since i don't open all my social media a lot :)

Here are writeups for some tasks from Oman AMAN ctf

# Tasks:
* **[Cryptography](#cryptography)**
1. [Go to Incomplete Password 100 points](#incomplete-password-100-points)

2. [Go to Forgot the password 80 points](#forgot-the-password-80-points)

3. [Go to Multi-shots 60 points](#multi-shots-60-points)

4. [Go to Sound code 40 points](#sound-code-40-points)

# Cryptography:

# Incomplete Password 100 points:

They captured the following sha1 hash, which was used by the hacker:

827d1057ad7258b180efca5e9cc25795a1a5f622

They looked further into the network logs and only found the first few plain characters of the password the hacker used:

Om@nYg

Use your programming skills to brute-force the remaining part of the password, knowing that the system the hacker was able to compromise has the following password requirements:

- Password must not exceed 9 characters
- Must contain lower & capital case characters
- Alphanumeric characters are allowed
- Only * & @ % ^ + _ special characters are allowed
- The password is the flag.

Here we can build a program from scratch that calculates the sha1 hash of *Om@nYg* and compare it to the given hash, if true then SuGoi!!
if not then add characters that follow the password policy given in the task.

In my case I'm a bit lazy to complete such a code, but you can always have a glimpse to Madiox writeup code which is well ordered and neat👌✨..
However, stone solved it in 2 ways:
* using John:

When attempting to *Brute Force*, we start 1st by adding one input, then if we added all (allowed) inputs and yet didn't reach the matching hash, then we add 2 inputs to the (raw) password that we are brute forcing.

When i tried brute forcing, i started with 3 inputs xD and i got nothing even tho it took more time, so i did it with 2 inputs and i got the following:

![Screenshot 2021-08-18 222831](https://user-images.githubusercontent.com/59108199/132400818-87cf9b58-ff66-442e-9634-bc044bffffc7.png)

* using hashcat:

By following the same approach, but just a little difference regarding the tool we are using:

![Screenshot 2021-08-18 222955](https://user-images.githubusercontent.com/59108199/132400879-e43614dd-4680-48df-8d06-92cccb004182.png)

Stone Aint giving you the result in a golden plate, but in a wooden one >:)

![200w (1)](https://user-images.githubusercontent.com/59108199/132441686-6f15287a-6213-407e-8151-6908089801ce.gif)


and as you've noticed, we didn't need the hash which they gave us in the task because it was easy since the brute forcing was short with maximum of 3 inputs! otherwise we would probably need to do the code to solve this task. and it's an easy one in python, but stone was lazy to do it. [CHECK OUT MADIOX]

# Forgot the password 80 points:
I have a ZIP file.  Can you brute-force the password to decompress it?

This task is an easy one as well, having some tricks made it enjoyable xD

Anyway, here we have a zip file that needs a passw, you may build a python program to
solve it too, but again John saves the day <:)

but before using John tools, it's a better practice to inspect the content of the zip folder. Here I used 7z which is an ideal tool to handle .zip folders:

7z l {location of the .zip folder}

like:

7z l flag.zip

we found out there is only 1 file called flag.txt so to extract it you gotta study how 7z tool work >:)

Now we move on.. John has active passw cracking tools to brute force archives such as .zip and .rar folders, so we gonna use zip2john tool to get the pw of the given zip folder:

![Screenshot 2021-09-07 002712](https://user-images.githubusercontent.com/59108199/132403849-600c39b7-4a90-461f-a249-b6916f138b59.png)

And I saved the hashes in a .hash file called zip.hash, noice so far? :) cool

and you probably know why we need to store the hashes in zip.hash that I made... because they are still (hashes) and we need to crack them, so we gonna use John *himself* ths time to do da cracking, and you know da way >:)

well, this time i will give it away to show off some generosity:

![hqdefault (1)](https://user-images.githubusercontent.com/59108199/132442114-b4ad14fc-4203-46b7-bfe1-6f4381205503.jpg)

![Screenshot 2021-09-07 002617](https://user-images.githubusercontent.com/59108199/132404493-db2bb54d-6ab2-4798-bff2-b170d4287884.png)

We can see the passw of the .zip folder in the middle of the output, and if buy mistake you closed the terminal, no worries coz this John stores his cracked corpse in a safe place, just use his patterns such as --show  to reveal them again without cracking again.

now we got the flag.txt file, and it has the following:

![Screenshot 2021-09-07 002313](https://user-images.githubusercontent.com/59108199/132405137-5b23f79f-9bb2-4baa-aef3-79f50f204a35.png)

as seeen, the 9th line of the file has a looooong string, and it asks you to pull out the "special characters" since they form up the final flag! 

we need a little program to do the job faster. After using my little python program, I found out that there is no speacial characters or numbers, so after looking thoroughly (and stone actually checked it manually), I saw that most chars are small-case, so i modified my little program to get out only upper-case chars, and SuGoi! xD

The python code looks like:
```
import os

entries = os.listdir('/home/stone/Downloads/AMANctf/scattered/')

with open('/home/stone/Downloads/AMANctf/forgotThePW/flag.txt', 'r') as f:
    lines = f.readlines()
f.close()

new_string = ''.join([c for c in str(lines[9]) if c.isupper()]) #in this case if the the char isn't small then it is special because most characters are small.

print(new_string)
```

# Multi-shots 60 points:

To improve security, a script kiddy claimed that he made his password very hard to recognize.  Can you help us reveal the password from the attached file?

well, here they gave us a file called ciphered.___ , basically after using the command *file ciphered.___* it's just a ASCII text file, but its contents is...

![Screenshot 2021-09-07 125423](https://user-images.githubusercontent.com/59108199/132438365-2798858a-e719-4ce0-8e99-2bc8a3604a2b.png)

that is how a binary text looks like!

there are many ways to do it, some veteran people use bash script tools like _perl_ or _xxd_ or their own codes, but stone used something veryyyy lazy easy, which is the internet.. <:)

You can find many websites to do a lot of things for you especially when you try to solve CTFs, but as we ALWAYS say, it is a **great** practice to not rely on these easy ways (and the talk is meant to me as well 🗿💧 ) 

![hqdefault](https://user-images.githubusercontent.com/59108199/132442478-f5c09a92-15bc-4a1f-8cc4-d8e1daa1fceb.jpg)

Anyway, i've used the following website since you can configer your preferred inputs & outputs as you wish: https://cryptii.com/pipes/binary-decoder

![Screenshot 2021-09-07 125523](https://user-images.githubusercontent.com/59108199/132439829-d7ed4fd6-f2d9-4025-9aab-58e3a4e359fa.png)

as we see in the right side of this online tool (the plain text side), the binary was decoded to give us another unreadable text, this text seems to be encoded with base64 (_you will get used to the different types of encryptions with time as you go by solving many CTFs, but if you want an easy tip, just copy and paste the encoded text on google and it mostly gives you a hint in the google results on the encoding method used for your text_). Moving on, I decrypt the base64 text using another online tool just to make sure: https://www.base64decode.org/

![Screenshot 2021-09-07 125621](https://user-images.githubusercontent.com/59108199/132440494-63eb601d-c41a-45d9-b834-6c285fe402d3.png)

as seen in the output down in the pic, this time we have a long sequence of digits and some letters they look like sha1 or some similar thing but not really. playing and trying around on them using the 1st online tool that I used, it turned out it's in hexadecimal, so: 

![Screenshot 2021-09-07 125743](https://user-images.githubusercontent.com/59108199/132441061-a58177af-fe59-4083-ae65-1cb9066ce3ac.png)

and the plain text of it is a set of chars with numbers, this reminds me of the ASCII table, you can check the ascii table down from https://www.dcode.fr/ascii-code and replace the number above to result in the plain final flag:

![Screenshot 2021-09-07 125804](https://user-images.githubusercontent.com/59108199/132441657-44c10f02-9796-4405-a6b4-fd4946740b0a.png)

![download1](https://user-images.githubusercontent.com/59108199/132441992-20e379aa-f75a-4458-9601-0bdc582d1515.jpg)

# Sound code 40 points:
You were able to intercept a secret message over a telecommunication channel between 2 hackers.  We need to decode this message.  Can you help us?

Here they gave us a .wav file (voice media file). After playing it, it has no words! just "toot toooot" like ON-OFF signal tones, so it's clear that it is a morse code.

![53hqmz](https://user-images.githubusercontent.com/59108199/132520312-023817cf-a519-469a-b63b-7ca85a6b97f6.jpg)

I googled for an online tool to do the job, just to shorten the time, take this link https://morsecode.world/international/decoder/audio-decoder-adaptive.html:

![Screenshot 2021-09-07 130846](https://user-images.githubusercontent.com/59108199/132521915-331efbfb-ae1c-41f3-95e5-79b67fd337c4.png)

After uploading the .wav file, waited a little until the tool listens to the whole voice, and it gave me a row text (most likely, Hexadecimal) so i decoded it using the usual online tool https://cryptii.com/pipes/binary-decoder:

![Screenshot 2021-09-07 130955](https://user-images.githubusercontent.com/59108199/132521824-12a8dd18-5af3-4378-bfde-84c1e59d89c9.png)



