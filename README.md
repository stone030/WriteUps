The writeups here are presented for education purpose only :)
for any comment you can either send it here or to Stone's social accounts directly, though i sometimes take days to reply since i don't open all my social media a lot :)
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

well, this time i will give it away:

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
