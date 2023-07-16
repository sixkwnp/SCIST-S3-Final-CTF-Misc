## Walkthrough >> [SCIST S3 Final CTF Platform](https://final.ctf.scist.org/)
> [HackMD Misc Official Writeup by sixkwnp](https://hackmd.io/@sixkwnp/ryxZqDZK3)
> ~~```不要使用假帳看 Hint 啦，不然我要怎樣釋出更多的提示QQ```~~

## Deepeye
`Format stego`,`Image stego` 
> I am not the script kiddle... But I'll be the master of psychic!
> 
> FLAG Format: `SCIST{.*}`
> [name=Author: sixkwnp]
> [time=Fri, Jul 7, 2023 4:08 PM]

Hint1: ||網路有現成工具可解題，請仔細聽  .wav 音頻，裡面有非常明顯的提示；並且，題目共須解開三層關卡。||

---
### Solution
In the `Deep.wav`file, It had mentioned three computer tools or tricks repeatedly. Two of them, `Deepsound` and `Magiceye` (one way to steghide the image(Stereogram)), are the tools we need to solve this challenge.
> ex: 
> ![](https://hackmd.io/_uploads/ByaazWPYn.png)
1. Deepsound (.wav steghide a **PDF file**) ->
2. Password is ||`SCIST`|| which is everywhere in the first line of .wav sound. 
Then we send `Deep.wav` to Deepsound and get a PDF file. ->
>![](https://hackmd.io/_uploads/H1Jy_evY3.png)
>
> ![](https://hackmd.io/_uploads/HyuEulDt3.png)
3. And we will be notified that `First.pdf` is damaged.
> ![](https://hackmd.io/_uploads/Hyp1txDYh.png)
4. The correct path is to check whether this file is damaged or not.
If it isn't broken, we have to find ways to open it. (There're also PDF repair challenges in Misc sometimes though) ->

> ![](https://hackmd.io/_uploads/ByiAQ6PYn.png)
5. After we tried to use `hexdump` to check PDF whether broken or not, we'll see the first line.
```bash=
50 4B 03 04 | PK.. #it's the header of zip (Phil Katz)
```
6. So we have 70% confidence that it's a `fake .pdf file`, it should be a `.zip` file. 
Addtionally, `Binwalk`/ `Hexeditor`(For instance, `HxD` is a great freeware to utilize and solve) can also find the real Filename Extension or something steghiden & compressed inside.
> `binwalk`ex: 
> ![](https://hackmd.io/_uploads/SyiHw4_Yn.png)
> (p.s: there's a possibility that we should change header to `25 50 44 46 2D` (.pdf header) and tail of the file, but this challenge kinda few 'cause it's somewhat uncertain & need to guess)
7.  Got `Final_H4ck3R.jpg` image and a file called `Second`.
> ![](https://hackmd.io/_uploads/SJISmHOth.png)
> 
> In`Second`file:
> ![](https://hackmd.io/_uploads/B1ObESuK2.png)
> 
> `Final_H4ck3R.jpg`:
> ![](https://hackmd.io/_uploads/B17GHSdt3.png) 
8. In the audio of `Deep.wav` (mentioned `Magiceye`) and `Second` (mentioned `Magic!`) hint twice about `Magiceye` this trick.
9. There're online solver related to `Stereogram` or `Magiceye` already (Automatically detect the format). example:
    - [Magiceye(Autostereogram) Solver](https://piellardj.github.io/stereogram-solver/)
    - [Magiceye(Autostereogram) Alternative Solver](https://magiceye.ecksdee.co.uk/)
        - ![](https://hackmd.io/_uploads/Sy3rPr_th.png)
* JPG steghide used Magiceye (Stereogram) to hide flag.
 * **Final FLAG : ||SCIST{YoU_4r3_K1N6_oF_T31eP47HY_XD}||**
![](https://hackmd.io/_uploads/rym75PZFh.png)
---

## Manatsunoyoru no Attack
`Forensics`,`Pcap analysis`
> 屈原既放，遊於江潭，行吟澤畔，顏色憔悴，形容枯槁。見一鯊，欲用線逮之，疑是身心俱疲，有黑色高級車，不幸追之。其一曰三浦，庇年幼者，俱攬其責。高級車主，暴力團員谷岡也，見此提要求數條，乃為……
> ![](https://hackmd.io/_uploads/H1v0_BrFn.png)
> 
> FLAG Format: `SCIST{.*}`
> [name=Author: sixkwnp]
> [time=Fri, Jul 7, 2023 4:08 PM]
Hint: ||Pcap 檔包含文字對話與混淆的亂碼，請活用 Wireshark  Filter 以過濾封包，或者使用各式內建功能找到關鍵訊息；另外，要取得必需的 key 時，請觀察特定 Packet 會話收送之 IP 變化，本題目總共存在三層關卡。||
---
### Info
By using the various built-in function in the `Wireshark` or `Tshark`, we can easily identify which kinds of datas are what we want. 

For example, `Wireshark filter` can help us to difference different packets of **protocols**, and it's helpful for these "fairly organized" packet flow challenge; also, `Conversations` function in the Wireshark is a useful tool whether to **solve the CTF challenge** or **detect the malicious network traffic in server rooms scenario.**

ex:
> ![](https://hackmd.io/_uploads/B1q5zdFK3.png)
---
> Location of `Conversations` function:
>![](https://hackmd.io/_uploads/ByUF1uFth.png)
---
If you want to know more about 
1. `Network Packet Analysis` 
2. `Monitoring Network Traffic` 
3. `Solving Pcap Forensics CTF Challenge`
4. `Network Management`

I had recorded some `Wireshark` entry video for training *Network Manager & CTF player*, welcome to check it: 
### - [Wireshark 教學 - 封包分析實作 | 系統管理培訓](https://youtu.be/Bhs173xh4fk)
---
### Solution
There're many ways to solve my second challenge, I will choose two way to explain it.

**Slow Method (newbie)**

1. Use filter for differencing different protocols   to find which traffic seem more malicious; If we Scroll Down, we'll see there're a lot of `ARP(Broadcast)` than any others like **`ICMP | TCP | UDP`**. But that's not enough to get the `FLAG`.

    If you have experience related to network field, you would know that transferring data while reaching websites often uses `HTTP protocol`.
    
    (P.S.) Maybe you would notice I put MORE THAN THOUSANDS packet to interrupt anyone to find the FLAG using `string -a` ;)
    >Like this:
    > ![](https://hackmd.io/_uploads/ryJ6DutK3.png)
    >
    > Or like this:
    > ![](https://hackmd.io/_uploads/BJONdOKtn.png)
    >
    > Even like this:
    > ![](https://hackmd.io/_uploads/SyHaktYF3.png)

    Eastern Egg XD:
    > ![](https://hackmd.io/_uploads/SkpPlttF2.png)

2. If using `TCP` u might see some interesting things. 
But firstly let us check packet under **FLAG????** and **SCIST{** :

    There's a hint:
    > ![](https://hackmd.io/_uploads/SJcs-YKFn.png)
    > ![](https://hackmd.io/_uploads/ryYKM5tt3.png)
3. After that or using **`tcp`** filter, we'll easily see some malicious conversation.
It's about a hacker called ''Senpai'' attacking the computer of ''MiURa''.

    Here's some image, you can watch detailed story in `Black_Luxury_Car.pcap`:
    > ![](https://hackmd.io/_uploads/HyIZQKKYh.png)
    
    - The green `TCP` stream means `Client Hello Packet`, first packet of Three-Way Handshake (三向交握)
4. Additionally, kinda a fun thing is that you would find IPs related to challenge or being modified are **`192.168.5.55` & `192.168.244.22`** -> represented `Senpai` and `MiURa`.
    
    (這裡需要道個歉，IP 沒有設定好，在故事部分會一直 Decrease，開賽時沒發現到這點沒修改到，因此以**第一個發送的封包**為準，但大部分還是可以從這判斷，從 **`Conversations`** 較為明顯) 
    > **封包發送數量明顯較多**:
    >
    > ![](https://hackmd.io/_uploads/BkZgDKtKn.png)

5. SeNPai smiled and said: "OK, then. The first thing you need to do is to know this **PDF**..But **where's the key**??
    > Two packets including all data of PDF **below**; also, if you find this thing, you know you should find the **key**:
    > 
    > ![](https://hackmd.io/_uploads/SJinSFFKh.png)
    ---
    > ### PDF:
    > ![](https://hackmd.io/_uploads/S1uXtKtt3.png)
    >
    > ![](https://hackmd.io/_uploads/H1q02tKFn.png)
    ---
    > ### Null packet:
    > ![](https://hackmd.io/_uploads/HyJcFYKY3.png)
    > 
    > ![](https://hackmd.io/_uploads/rk4jYtKF2.png)
    ---
    > ### **Hint:**
    > ![](https://hackmd.io/_uploads/SkRpKtYKh.png)
    ---
    ### Knowledge requirements
    > Black - **Packet Header** (include the `info of packets`, `length`, `protocols`, `5 levels of OSI`) 
    > Blue (The data part which I select) - **Hex / Data** transferred by packet

    **So we should export the selected part:**
      
      -  ![](https://hackmd.io/_uploads/HyQNqtYFh.png)
6. Exporting object

    (1). Method one
    > ![](https://hackmd.io/_uploads/rkOkCFtF3.png)

    (2). Method two
    > ![](https://hackmd.io/_uploads/BJOYAKKKn.png)
    > 

    (3). Method three
    > ![](https://hackmd.io/_uploads/SyL1WcFYn.png)

        Method(2) you will need to delete space like this http://www.esjson.com/delSpace.html  
7. 
    > Remember to remove **packet header** many `00 00 00 00`**in the tail of second packet**.

     (after `45 4f 46 0a`)
    ![](https://hackmd.io/_uploads/HJDlg9Kth.png)
8. Hex to file (or use 010editor, Notepad++, HxD...):
    > ![](https://hackmd.io/_uploads/SJf5ZqFK3.png)
9. It's encrypted:
    > ![](https://hackmd.io/_uploads/BJ27zcKt2.png)
10. Tring to use `Seipai` and `MiRUa`'s IPs to find the `key of PDF`.
    > ![](https://hackmd.io/_uploads/SJDem9tK3.png)
    > BOOM!!! The last conversation packet by them stored 
32 characters key **`2a9d119df47ff993b662a8ef36f9ea20`** has found by you!

11. To prevent the **Key** leaked by the `string -a`, I used Base64 to encode it. 
(Maybe use cipher identifier to decode?)
    > ![](https://hackmd.io/_uploads/ryWN49YK3.png)
12. Decoding Result:
    > ![](https://hackmd.io/_uploads/ryc4gdZ5n.png)

**Fast Method**
1. Just use **`Conversation`**, you would not only find the `story` 
(ip.addr of Encrypted PDF is `0.0.0.0`, but still, easier to find when you saw `Packet Sequence Number`) 
also quicker to see the `key` in the last packet.

    > ![](https://hackmd.io/_uploads/rk-Mw5FYn.png)
 * **Final FLAG : ||SCIST{pc4p_4n4lyS15_1snT_h4rd_W17H_Bl4CK_LUXURY_C4R}||**
---

## Entry Forensics
`Disk forensics`,`MEM forensics`
> In Disk Forensics challenges, participants are typically presented with a disk image or a collection of files, which they must examine and extract relevant information from. Revolving around investigating and analyzing data stored on computer hard drives or other storage devices to uncover valuable information, such as evidence of malicious activity, data breaches, or unauthorized access. 
>
>You need to utilize their expertise in various CTF abilities of digital forensics, including identifying hidden files or directories, data recovery techniques, analyzing file system structures, cracking  encrypted content, or reconstructing a sequence of events leading up to a particular incident.
> 
> Challenge Background: 最近一家跨國公司遭遇了一次嚴重的資料外洩，該公司的 IT 部門從受影響的電腦之一提取了一個 Disk image，他們懷疑其中包含了與外洩事件相關的重要證據；作為一名數位鑑識調查人員，您的任務是分析這個 Disk image，並找到以 FLAG `SCIST{a-z_A-Z_0-9}` 表示的關鍵信息，以協助調查工作。
> 
> [name=Author: sixkwnp]
> [time=Fri, Jul 7, 2023 4:08 PM]

> Hint1: ||用工具或指令對 .mem dump 出 FLAG 資料夾的位置||

> Hint2: || FLAG 為 .png 檔案||

---
### Solution (Many ways / hints)

1. Use `FTK Imager`, `Autopsy` or `other Forensics tool` to open it, we don't introduce and teach the funtions of these tools step by step here, there're a lot of tutorials on the internet.
(p.s. walkthrough **lots of disk forensics/mem forensics** will be helpful for 
utilizing these tools)

        [root] means the system disk. ex: C:\ in the windows system
> This a **Windows image**, just see the name of dir.
> ![](https://hackmd.io/_uploads/SkuKxsFt3.png)
2. 
>![](https://hackmd.io/_uploads/SyBaMiKKn.png)
3. Fail to generate Malicious_Image.ad1... -> **.txt is the log**
You can think that the company tried to generate this disk image for DFIR, but the process was broken by hacker's intrusion.
> ![](https://hackmd.io/_uploads/H19HSjYFn.png)

    Hints
    - C:\Users\sixkwnp\Documents
    - C:\Users\sixkwnp\Contacts
    - C:\Users\sixkwnp\Videos
- ![](https://hackmd.io/_uploads/HyN44oKY3.png) 

4. It gave you a file called `SCIST.Entry.forensics.txt` (find detail below), and `SCIST_address` hint the address of **.MEM forensics file**:
    ```
    C:\Windows\SysWOW64\Recovery\Company\SCIST.fixed.mem
    ```
> :::spoiler
> Memory Forensics
>
> Introduction
> Memory analysis or Memory forensics is the process of analyzing volatile 
> data from computer memory dumps. With the advent of â€œfilelessâ€ malware, it 
> is becoming increasingly more difficult to conduct digital forensics 
> analysis. Memory analysis not only helps solve this situation but also 
> provides unique insights in the runtime of the systemâ€™s activity: open 
> network connections, recently executed commands, and the ability to see 
> any decrypted malicious file. There are plenty of traces of someone's 
> activity on a computer, but perhaps some of the most valuble information 
> can be found within memory dumps, that is images taken of RAM. These dumps 
> of data are often very large, but can be analyzed using a tool called 
> Volatility provided by the Volatility Foundation.
> 
> Volatility is an open-source memory forensics framework for incident 
> response and malware analysis. This is a very powerful tool and we can 
> complete lots of interactions with memory dump files, such as:
> 
> - List all processes that were running.
> - List active and closed network connections.
> - View internet history.
> - Identify files on the system and retrieve them from the memory dump.
> - Read the contents of notepad documents.
> - Retrieve commands entered into the Windows Command Prompt.
> - Scan for the presence of malware using YARA rules.
> - Retrieve screenshots and clipboard contents.
> - Retrieve hashed passwords.
> - Retrieve SSL keys and certificates.
> :::
> [name=SCIST.Entry.forensics.txt]

 **SCIST_address**:
> ![](https://hackmd.io/_uploads/By4B34b52.png)
[name=C:\Users\sixkwnp\Documents]
---
**SCIST.Entry.forensics.txt**:
> ![](https://hackmd.io/_uploads/HkfCSVbc2.png)
> [name=C:\Users\sixkwnp\Documents]

> There's also a hint
> ![](https://hackmd.io/_uploads/rkkY5NZ9n.png)
>
> [name=[root] -> C:\]
5. As you follow the hint, you should use your Mem Forensics skills/tools to solve **.MEM file** hidden challenge; but first, let us find hints / stories in Directory that mentioned in `Malicious_Image.ad1.txt`:
    ```
    - C:\Users\sixkwnp\Contacts
    - C:\Users\sixkwnp\Videos
    ```
- First story that later will use in solving final challenge
> :::spoiler
> [17:35] <phy114c1<3r>: I have some news for you. I found a way to hack into the confidential computer of the company we are targeting.
> [17:36] <phy114c1<3r>: it's not easy, but it's possible. You need to use a special software that I developed, and a code word that I will give you.
> [17:37] <phy114c1<3r>: are you interested?
> 
> [17:40] <zal0>: of course I am! This is what we have been waiting for. How did you do it?
> 
> [17:41] <phy114c1<3r>: I can't tell you the details here, it's too risky. But I can show you the software and the code word when we meet.
> [17:42] <phy114c1<3r>: the software is called Zephyr, and it can bypass the security system of the computer. The code word is ZÌ´Í†Ì†Ì¾Ì†Í„Í„Ì„ÍŠÍ„Í‚ÌƒÍÌ¾Ì£Í–Ì¦Ì²Ì­Ì™Ì­ÌÌ©Ì¦ÌÌ—Í‰aÌ´Í›ÌÌÌ¿ÌšÌ¾Í€Ì…ÌšÌ“Ì‘ÌÍ‚ÌšÌ’Ì€ÍÌ–ÌŸÌžÌžÌ¬Ì¦Í™Ì°Ì±lÒˆÍÌÍŠÍÍÌ€Ì™ÍˆÌœÍˆÍ…Ì˜gÌ¶ÌŽÌ„Í—Ì‰Ì‘Ì¦Í–Ì°Ì¤Ì–Ì¬Ì™Ì©Ì¦oÌ´ÍÌ“ÌÍŒÌˆÌ€Í‹Ì€Í—Í—Í†Í‚Ì«ÌŸÌ®Ì²Ì­Í™Ì°ÌžÍ‰Í‰.Ì¸Í›Í‹ÌˆÌšÍÌ‘ÌŽÌ‡ÌÌŠÌÌÍ€Í—Ì¾Í‚Í“ÌÍ•Ì±ÍÌ Í•Ì¤ÍˆÍ™Ì¬ÌŸ.Ì¶Ì‚Í’ÌŒÍŒÍ€Í’Ì„ÌªÍÌ±Í™Ì¥ÍˆÌžÌ®Ì©Í…Í“Ì˜ÍŽÍÌœ.Ì¶ÍŠÍ€Ì¿ÍŒÌ¾Ì½ÌÌ†Ì¾Í“ÍˆÌ¬Í–Ì£Ì³Ì±Í•Í‡ÌªÌ³Ì™Ì˜Í•, just put the software into the confidential computer in ur company. 
> 
> [17:43] <zal0>: Zephyr and ZÌ´Í†Ì†Ì¾Ì†Í„Í„Ì„ÍŠÍ„Í‚ÌƒÍÌ¾Ì£Í–Ì¦Ì²Ì­Ì™Ì­ÌÌ©Ì¦ÌÌ—Í‰aÌ´Í›ÌÌÌ¿ÌšÌ¾Í€Ì…ÌšÌ“Ì‘ÌÍ‚ÌšÌ’Ì€ÍÌ–ÌŸÌžÌžÌ¬Ì¦Í™Ì°Ì±lÒˆÍÌÍŠÍÍÌ€Ì™ÍˆÌœÍˆÍ…Ì˜gÌ¶ÌŽÌ„Í—Ì‰Ì‘Ì¦Í–Ì°Ì¤Ì–Ì¬Ì™Ì©Ì¦oÌ´ÍÌ“ÌÍŒÌˆÌ€Í‹Ì€Í—Í—Í†Í‚Ì«ÌŸÌ®Ì²Ì­Í™Ì°ÌžÍ‰Í‰.Ì¸Í›Í‹ÌˆÌšÍÌ‘ÌŽÌ‡ÌÌŠÌÌÍ€Í—Ì¾Í‚Í“ÌÍ•Ì±ÍÌ Í•Ì¤ÍˆÍ™Ì¬ÌŸ.Ì¶Ì‚Í’ÌŒÍŒÍ€Í’Ì„ÌªÍÌ±Í™Ì¥ÍˆÌžÌ®Ì©Í…Í“Ì˜ÍŽÍÌœ.Ì¶ÍŠÍ€Ì¿ÍŒÌ¾Ì½ÌÌ†Ì¾Í“ÍˆÌ¬Í–Ì£Ì³Ì±Í•Í‡ÌªÌ³Ì™Ì˜Í•. Got it. When can we meet?
> ã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸ
> 
> [17:45] <phy114c1<3r>: tomorrow night, at the usual place. Be careful, and don't tell anyone else about this.
> 
> [17:46] <zal0>: don't worry, I won't. See you tomorrow, hacker buddy.
> 
> [17:47] <phy114c1<3r>: see you tomorrow.
> :::
> [name=confidential.cam]

> C:\Users\sixkwnp\Videos:
> 
> ![](https://hackmd.io/_uploads/BJ6i8Sbc2.png)
6. Check other dirs
```
C:\Users\sixkwnp\Contacts
```
> C:\Users\sixkwnp\Contacts:
> 
> ![](https://hackmd.io/_uploads/SyFAyrb92.png)
---
- **Hint twice about the location of .MEM file challenge** below
> C:\Users\sixkwnp\Contacts\Company Confidential:
> 
> ![](https://hackmd.io/_uploads/rkTvWSWqh.png)
---
- Second story that later will use in solving final challenge
> :::spoiler
> <Zoe> 2023-07-06 16:46:00
> I'm ready. Today is my last day at this company. Are you sure you've inserted the malicious software into the highly confidential computer?
> 
> <Andrew> 2023-07-06 16:46:15
> Don't worry, I've taken care of everything. Once you press the send key, it will trigger the malicious software and crash the entire system.
> 
> <Zoe> 2023-07-06 16:46:30
> That's great. I've been waiting for this day for a long time. This company and CEO Mattias deserve to be punished for all the unfair things > they've done to us.
> 
> <Andrew> 2023-07-06 16:46:45
> Yeah, they never value our work and contributions. They only exploit our labor and intelligence. We need to show them that we're not to be > messed with.
> 
> <Zoe> 2023-07-06 16:47:00
> Let's take action together then. I'm pressing the send key now. Goodbye, Andrew. I hope you find a better job.
> 
> <Andrew> 2023-07-06 16:47:15
> Goodbye, Zoe. I wish you all the best. We may never see each other again, but I'll always remember you.
> :::
> [name=Andrew\1.txt]

> C:\Users\sixkwnp\Contacts\Andrew:
> 
> ![](https://hackmd.io/_uploads/rJ0emSZch.png)
---
- **Hint third about .MEM challenge and some tools + challenges**
> :::spoiler
> Browsing History
> https://en.wikipedia.org/wiki/Memory_forensics
> 8.8.8.8
> https://github.com/apsdehal/awesome-ctf
> https://github.com/volatilityfoundation/volatility3
> :::
> [name=Zoe\1.txt && Zephyr\1.txt]

> C:\Users\sixkwnp\Contacts\Zoe || C:\Users\sixkwnp\Contacts\Zephyr:
> 
> ![](https://hackmd.io/_uploads/SJxUUSWqn.png)
7. And then we can go to **`C:\Windows\SysWOW64\Recovery\Company\SCIST.fixed.mem`** to solve Mem Forensics chanllenge.
- Use build-in funtion (FTK imager, Autopsy, other tools...) to **Export the .MEM file**
>![](https://hackmd.io/_uploads/SJXScHbq3.png)
    - ![](https://hackmd.io/_uploads/rkdue_bch.png)
8. Use `Volatility2`(Recommended -> more funtions for WIN), `Volatility3`, `strings -a | grep <keyword in stories>`,... to find where's the flag.

> ### For instance:

> ### [volatility2](https://github.com/volatilityfoundation/volatility/wiki/Volatility-Usage)
```
cd volatility
python2 vol.py -f SCIST.fixed.memi mageinfo   # identify the operating system
---
python2 vol.py -f SCIST.fixed.mem --profile=Win10x64 cmdscan   # CMD history
---
python2 vol.py -f SCIST.fixed.mem --profile=Win10x64 consoles  # CMD history alternatives
```
> ### [volatility3](https://volatility3.readthedocs.io/en/latest/)
```
cd volatility3
python3 vol.py -f SCIST.fixed.mem windows.cmdline.CmdLine    # CMD history(不完整)
---
python3 vol.py -f SCIST.fixed.mem windows.pstree   # List the Running Processes while the memory dump was taken
---
...
```
Just use `volatility2` or other tools to find **Powershell/CMD history**, we can easily find the hacker try below in the terminal.
```bash=
cd 'C:\Users\sixkwnp\Appdata\Local\Temp\'
mkdir 'Who is zal0 CASESENSITIVE'
cd '.\Who is zal0 CASESENSITIVE\'
mkdir 'Who is phy114ck3r'
cd '.\Who is phy114ck3r\'
mkdir 'Who is CEO'
cd '.\Who is CEO\'
mv C:\Users\sixkwnp\Appdata\Local\Temp\112f3a99b283a4e1788dedd8e0e5d35375c33747.png .
```
You also can `dump the file below` to get **Terminal(Powershell) history**
```
C:\Users\sixkwnp\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt.
```
9. Then we just go to `C:\Users\sixkwnp\Appdata\Local\Temp\`using Disk forensics tool
> Export this `Who is zal0 CASESENSITIVE.7z` file
>
> ![](https://hackmd.io/_uploads/rJREcDW92.png)
[name=C:\Users\sixkwnp\Appdata\Local\Temp]
### 
10. If you take a glance at two stories and `C:\Users\sixkwnp\Contacts`,
easily can understand who is `zal0`, `phy114ck3r` and `CEO`, respectively.
---
> ![](https://hackmd.io/_uploads/BJO-sDZq2.png)
> - key: ||Zoe|| (Case sensitive)
> 
> [name=Who is zal0 CASESENSITIVE.7z]
---
> ![](https://hackmd.io/_uploads/Bk-i2vbc3.png)
> - key: ||Andrew|| (Case sensitive)
> 
> [name=Who is phy114ck3r.7z]
---
> ![](https://hackmd.io/_uploads/SkVm6Pbq2.png)
> - key: ||Mattias|| (Case sensitive)
> 
> [name=Who is CEO.7z]
11. Successfully dump the flag!
    
![](https://hackmd.io/_uploads/S10sg_Z5n.png)
- ![](https://hackmd.io/_uploads/BkgGCPW9h.png)
 > [name=112f3a99b283a4e1788dedd8e0e5d35375c33747.png]
* **Final FLAG : ||SCIST{Vol4T1L17Y_C4N_do_4LL_Non53nse_M3MFoR3Ns1cs}||**

