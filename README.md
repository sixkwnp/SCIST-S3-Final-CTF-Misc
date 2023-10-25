## Contents:
- [Challenge](#challenge)
    - [HackMD Misc Official Writeup](https://hackmd.io/@sixkwnp/ryxZqDZK3)
- [Deepeye - Misc 1](#deepeye)
    - [Solution](#solution1)
- [Manatsunoyoru no Attack - Misc 2](#manatsunoyoru)
    - [Info](#info)
    - [Wireshark 教學 - 封包分析實作 | 系統管理培訓](https://youtu.be/Bhs173xh4fk)
    - [Solution](#solution2)
- [Entry Forensics - Misc 3](#entry)
    - [Solution](#solution3)

---

<h2 id="Challenge">Walkthrough</h2> 

### [SCIST S3 Final CTF Platform](https://final.ctf.scist.org/)
- [HackMD Misc Official Writeup by sixkwnp](https://hackmd.io/@sixkwnp/ryxZqDZK3)
> 
> ~~```不要使用假帳看 Hint 啦，不然我要怎樣釋出更多的提示QQ```~~

---

<h2 id="Deepeye">Deepeye</h2>

`Format stego`,`Image stego` 
> I am not the script kiddie... But I'll be the master of psychic!
> 
> FLAG Format: `SCIST{.*}`
> > - Author: sixkwnp
> > - Fri, Jul 7, 2023 4:08 PM
> 
> 

Hint1: 網路有現成工具可解題，請仔細聽  .wav 音頻，裡面有非常明顯的提示；並且，題目共須解開三層關卡。

---
<h3 id="Solution1">Solution</h3> 

In the `Deep.wav`file, It had mentioned three computer tools or tricks repeatedly. Two of them, `Deepsound` and `Magiceye` (one way to steghide the image(Stereogram)), are the tools we need to solve for this challenge.
> ex:
> 
> ![upload_cae57f0b35c351b3b1e3b0ef3e79e9a8](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/1a578435-bc0f-4316-afab-417681f5b9f9)
> 
>  
1. Deepsound (.wav steghide a **PDF file**) ->
2. Password is `SCIST` which is everywhere in the first line of .wav sound. 
Then we send `Deep.wav` to Deepsound and get a PDF file. ->
> ![upload_4d078310b23d9ac0d699790f07fc5697](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/aff226c8-388f-48a5-8aeb-c50272206a33)
>
> ![upload_2a5c8b5df7504334c883784a7318d4dd](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/f543081c-3366-4ae2-86ff-be7f1d64dd0d)
>
> 
3. And we will be notified that `First.pdf` is damaged.
> ![upload_5f699a853d55869ab7a516468c2d9a3f](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/2257c912-3afb-42d0-8ffe-77fd8846636f)
> 
>  
4. The correct path is to check whether this file is damaged or not.
If it isn't broken, we have to find ways to open it. (There're also PDF repair challenges in Misc sometimes though) ->
> ![upload_8d6cac6cde9c50db9b14496a722e9187](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/832d210a-5f69-4356-a9c2-b0232fb5a460)
> 
> 
5. After we tried to use `hexdump` to check PDF whether broken or not, we'll see the first line.
```bash=
50 4B 03 04 | PK.. #it's the header of zip (Phil Katz)
```
6. So we have 70% confidence that it's a `fake .pdf file`, it should be a `.zip` file. 
Addtionally, `Binwalk`/ `Hexeditor` can also find the real Filename Extension or something steghiden & compressed inside.
(For instance, `HxD` is a great freeware to utilize and solve) 

> `binwalk`ex:
> 
> ![upload_7e3e5a5a64f74baedbc76a852211f92f](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/d473e77a-df1a-4754-96f6-9e466407da2c)
> 
> (p.s: there's a possibility that we should change header to `25 50 44 46 2D` (.pdf header) and tail of the file, but this challenge kinda few 'cause it's somewhat uncertain & need to guess)
7.  Got `Final_H4ck3R.jpg` image and a file called `Second`.
> ![upload_a03f21836c9ce6079589de4ec84b6a51](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/683e7015-db2c-4119-ae47-42ba3e400cf1)

> In`Second`file:
>
> ![upload_468e92f3e72ca2d23037949d0d1f2d66](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/c287fb2e-5ad1-4be1-af1b-253e825b612f)
> 
> `Final_H4ck3R.jpg`:
> 
> ![upload_93e1716b4d5ad777016f453dc587c677](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/d99b4e21-dfe0-4100-b160-a7796be2bfeb)
> 
> 
8. In the audio of `Deep.wav` (mentioned `Magiceye`) and `Second` (mentioned `Magic!`) hinted twice about `Magiceye` this trick.
9. There're online solver related to `Stereogram` or `Magiceye` already (Automatically detect the format). example:
    - JPG steghide used Magiceye (Stereogram) to hide flag.
    - [Magiceye(Autostereogram) Solver](https://piellardj.github.io/stereogram-solver/)
    - [Magiceye(Autostereogram) Alternative Solver](https://magiceye.ecksdee.co.uk/)
        - Solved:    
        - ![upload_374fcdc76b80e72353011d386287c17d](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/0fc4faf1-a4a9-4575-bed2-8f693852669b)

### - **Final FLAG : SCIST{YoU_4r3_K1N6_oF_T31eP47HY_XD}**

---

<h2 id="Manatsunoyoru">Manatsunoyoru no Attack</h2> 

`Forensics`,`Pcap analysis`
> 屈原既放，遊於江潭，行吟澤畔，顏色憔悴，形容枯槁。見一鯊，欲用線逮之，疑是身心俱疲，有黑色高級車，不幸追之。其一曰三浦，庇年幼者，俱攬其責。高級車主，暴力團員谷岡也，見此提要求數條，乃為……
> 
> ![upload_403456274e941d23898f2368133fdece](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/3bbcbb5c-6570-4fcc-8e29-c9e376a52ffd)
> 
> FLAG Format: `SCIST{.*}`
> 
> > - Author: sixkwnp
> > - Fri, Jul 7, 2023 4:08 PM
> 
> 

Hint: Pcap 檔包含文字對話與混淆的亂碼，請活用 Wireshark  Filter 以過濾封包，或者使用各式內建功能找到關鍵訊息；另外，要取得必需的 key 時，請觀察特定 Packet 會話收送之 IP 變化，本題目總共存在三層關卡。

---
<h3 id="Info">Info</h3> 

By using the various built-in function in the `Wireshark` or `Tshark`, we can easily identify which kinds of datas are what we want. 

For example, `Wireshark filter` can help us to difference different packets of **protocols**, and it's helpful for these "fairly organized" packet flow challenge; also, `Conversations` function in the Wireshark is a useful tool whether to **solve the CTF challenge** or **detect the malicious network traffic in server rooms scenario.**

ex:
> ![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/3c3ec311-5774-4a59-9b72-1e852bab17e0)
> 
---
> Location of `Conversations` function:
> ![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/2b4f3dae-8437-4242-96ce-fc7338940072)
>
> 
---
If you want to know more about 
1. `Network Packet Analysis` 
2. `Monitoring Network Traffic` 
3. `Solving Pcap Forensics CTF Challenge`
4. `Network Management`

I had recorded some `Wireshark` entry video for training *Network Manager & CTF player*, welcome to check it: 
## - [Wireshark 教學 - 封包分析實作 | 系統管理培訓](https://youtu.be/Bhs173xh4fk)

<h3 id="Solution2">Solution</h3> 
There're many ways to solve my second challenge, I will choose two way to explain it.

**Slow Method (newbie)**

1. Use filter for differencing different protocols   to find which traffic seem more malicious; If we Scroll Down, we'll see there're a lot of `ARP(Broadcast)` than any others like **`ICMP | TCP | UDP`**. But that's not enough to get the `FLAG`.

    If you have experience related to network field, you would know that transferring data while reaching websites often uses `HTTP protocol`.
    
    (P.S.) Maybe you would notice I put MORE THAN THOUSANDS packet to interrupt anyone to find the FLAG using `string -a` ;)
    >Like this:
    > ![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/1c54f145-dec3-4e81-aca7-bdf1a3a58053)
    >
    > Or like this:
    > ![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/d606079f-8ce0-4f9e-8a33-46d47eebd22f)
    >
    > Even like this:
    > ![upload_d7b5b59acd8abaf4be94d6a24c489bbc](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/dad7e21b-c57b-4bbb-bf2b-4e85d3d10eef)
    >
    > 

    Eastern Egg XD:
    > ![upload_e3f154aa461e91406e282ce0ad7d7133](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/cdcc8627-eba5-445f-afaf-ba4932f6d4cc)
    > 
    > 

2. If using `TCP` u might see some interesting things. 
But firstly let us check packet under **FLAG????** and **SCIST{** :

    There's a hint:
    > ![upload_3856d0adc42771a2b4f23867abad9b41](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/1516c5fd-d4f4-470d-bf21-44402055df79)
    > 
    > 
    > ![upload_060e233ddd515ab5e62c8b7927ef462b](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/50a84cc5-ca00-48f6-83c2-89c7d5d95b08)
3. After that or using **`tcp`** filter, we'll easily see some malicious conversation.
It's about a hacker called ''Senpai'' attacking the computer of ''MiURa''.

    Here's some image, you can watch detailed story in `Black_Luxury_Car.pcap`:
    > ![upload_59b5a2f85d4e5bf8d5aa3d1b9c5b1783](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/d7e658cd-207d-4de8-b809-9e93f134e61e)
    > 
    > 
    
    - The green `TCP` stream means `Client Hello Packet`, first packet of Three-Way Handshake (三向交握)
4. Additionally, kinda a fun thing is that you would find IPs related to challenge or being modified are **`192.168.5.55` & `192.168.244.22`** -> represented `Senpai` and `MiURa`.
    
    (這裡需要道個歉，IP 沒有設定好，在故事部分會一直 Decrease，開賽時沒發現到這點沒修改到，因此以**第一個發送的封包**為準，但大部分還是可以從這判斷，從 **`Conversations`** 較為明顯) 
    > **封包發送數量明顯較多**:
    >
    > ![upload_aa00b7cde4d58da0b4d29052348fc05f](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/fb20b0d9-bb8e-4fa8-b48c-59a401d1c222)
    > 
    > 

5. Two packets including all data of PDF **below**; also, if you discover this thing, you will know you should find the **key**.
    > SeNPai smiled and said: "OK, then. The first thing you need to do is to know this **PDF**..But **where's the key**??
    > 
    > ![upload_cc5077441412770cf4312011b6a914aa](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/f81e1b86-49d8-4d3e-b21d-1e838e508785)
    > 
    ---
    Following packets - 
    > ### PDF:
    > ![upload_53bbad0f896f7a3b8d55f50b98633c74](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/bb1c0cde-9e4f-455a-8492-8b8c69944d54)
    >
    > ![upload_0bc3a35f281f55b6ac7c7df14dd70251](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/67050e93-258b-45d8-8b90-fede295c704b)
    > 
    ---
    > ### Null packet:
    > ![upload_9e6db6a9a1bc86eb20a0a88d9d5387bf](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/ff6c81b3-7519-4479-992c-57a5d4fc0e41)
    > 
    > ![upload_ef067092421b3fdece7e3ec7d4f8e2e0](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/dbd51246-5f55-4ef6-ba43-7b0bb8f5ecd0)
    > 
    ---
    > ### **Hint:**
    > `Plz combine the HEX of PDF`
    > ![upload_9091c5a803d96662fa54188ef637b030](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/80f45422-833d-4e54-9484-4cd8498d947f)
    > 
    > 
    ---
    ### Knowledge requirements
    > Black - **Packet Header** (include the `info of packets`, `length`, `protocols`, `5 levels of OSI`)
    > 
    > Blue (The data part which I select) - **Hex / Data** transferred by packet

    **So we should export the selected part:**
      
      -  > ![upload_edfbab28c93894d6aa6238bd1d11bfe2](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/da53c5ed-4639-46dc-8b84-cd9df7e87fc7)
         
6. Exporting object

    (1) Method one
    > ![upload_d2b03178a8b48233ae2032ffa14e7103](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/980bc636-cab0-4142-9ece-1a19020c0e99)

    (2) Method two
    > ![upload_ec144ac2c57c4b6d81b683d0acea0955](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/215a2c63-a00b-4a47-a8bc-a06c87ee6af2)

    (3) Method three
    > ![upload_38b48e497609eb132991cdc2ba3a6777](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/b6d39e1f-5371-482a-97fa-30782fbe8c63)

        Method(2) you will need to delete space using like this one http://www.esjson.com/delSpace.html  
7. 
    > Remember to remove **packet header** many `00 00 00 00`**in the tail of second packet**.
    > 
    > (after `45 4f 46 0a`)
    > 
    > ![upload_3a9ff720c206fa8622f9b059f7a7ac85](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/b96bab37-3c11-47d1-935f-eb9de2d169de)
8. Hex to file (or use 010editor, Notepad++, HxD...):
    > ![upload_d3592080a26016d8a2e52be31182f4bd](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/eb4d46d2-bc5d-4650-926e-5c39adc0da2c)
9. It's encrypted:
    > ![upload_1689b27c0d7360a114c9ed978b2fc738](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/47d4e652-daad-4451-acb3-7a2dff0bfcc6)
10. Tring to use `Seipai` and `MiRUa`'s IPs to find the `key of PDF`.
    > ![upload_8b02d10e94257edee9911f75b8525be1](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/9cc5269e-7975-4c31-a042-53f5b2d3a825)
    > 
    > BOOM!!! The last conversation packet of them stored 
32 characters key **`2a9d119df47ff993b662a8ef36f9ea20`** has found by you!

11. To prevent the **Key** leaked by the `string -a`, I used Base64 to encode it. 
(Maybe use cipher identifier to decode?)
    > ![upload_95e664c9fae651065af1aae3b19618d4](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/38e3bf99-68ba-49fc-9fae-8f31c081dabd)
    > 
12. Decoding Result:
    > ![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/9f3886f6-4079-4c05-a8c5-277b2d0e89fc)
    > 

**Fast Method**
1. Just use **`Conversation`**, you would not only find the `story`, also quicker to see the `key` in the last packet.
(ip.addr of Encrypted PDF is `0.0.0.0`, but still, easier to find when you saw `Packet Sequence Number`) 

    > ![upload_f1c73d1a041364a63454184a40b4845e](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/fad47ad8-dfa2-4e21-8bb4-b6881e6ddae2)
    > 

### - **Final FLAG : SCIST{pc4p_4n4lyS15_1snT_h4rd_W17H_Bl4CK_LUXURY_C4R}**

---

<h2 id="Entry">Entry Forensics</h2>

`Disk forensics`,`MEM forensics`
> In Disk Forensics challenges, participants are typically presented with a disk image or a collection of files, which they must examine and extract relevant information from. Revolving around investigating and analyzing data stored on computer hard drives or other storage devices to uncover valuable information, such as evidence of malicious activity, data breaches, or unauthorized access. 
>
>You need to utilize their expertise in various CTF abilities of digital forensics, including identifying hidden files or directories, data recovery techniques, analyzing file system structures, cracking  encrypted content, or reconstructing a sequence of events leading up to a particular incident.
> 
> Challenge Background: 最近一家跨國公司遭遇了一次嚴重的資料外洩，該公司的 IT 部門從受影響的電腦之一提取了一個 Disk image，他們懷疑其中包含了與外洩事件相關的重要證據；作為一名數位鑑識調查人員，您的任務是分析這個 Disk image，並找到以 FLAG `SCIST{a-z_A-Z_0-9}` 表示的關鍵信息，以協助調查工作。
> 
> > - Author: sixkwnp
> > - Fri, Jul 7, 2023 4:08 PM
> 
> 

Hint1: 用工具或指令對 .mem dump 出 FLAG 資料夾的位置
Hint2: FLAG 為 .png 檔案

---
<h3 id="Solution3">Solution</h3>

`(Many ways / hints)`
1. Use `FTK Imager`, `Autopsy` or `other Forensics tool` to open it, we don't introduce and teach the funtions of these tools step by step here, there're a lot of tutorials on the internet.
(p.s. walkthrough **lots of disk forensics/mem forensics** will be helpful for utilizing these tools)

        [root] means the system disk. ex: C:\ in the windows system
> This a **Windows image**, just see the name of dir.
> ![upload_5884992e376b9d781968ad9b73f21605](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/4ac4d046-6a74-4d61-920d-ab823c28a2f8)
> 
2. 
> ![upload_37d4b118364d07d54daad36f43fe8a97](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/c8d7e011-2c22-4c4d-9764-703e6bd6ebc4)
> 
3. Fail to generate Malicious_Image.ad1... -> **.txt is the log**
You can think that the company tried to generate this disk image for DFIR, but the process was broken by hacker's intrusion.
> !![upload_b6266c384069287eeeedc05eb5022e35](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/46be9d9d-8537-4eb5-b88c-543d8b249012)
> 

    Hints
    - C:\Users\sixkwnp\Documents
    - C:\Users\sixkwnp\Contacts
    - C:\Users\sixkwnp\Videos
- > ![upload_510933380205500e623cb8b64e8f2a50](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/6fabf9f7-d35a-4887-98f9-b61799e8034a)


4. It gave you a file called `SCIST.Entry.forensics.txt` (find detail below), and `SCIST_address` hint the address of **.MEM forensics file**:
    ```
    C:\Windows\SysWOW64\Recovery\Company\SCIST.fixed.mem
    ```
    
**C:\Users\sixkwnp\Documents:**
![upload_3c419f098fb224d0c3641296ccccb659](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/a9668ee3-78fa-4793-b12c-f5ea403646ae)

> **Memory Forensics**
>
> **Introduction**
> **Memory analysis or Memory forensics is the process of analyzing volatile 
> data from computer memory dumps. With the advent of â€œfilelessâ€ malware, it 
> is becoming increasingly more difficult to conduct digital forensics 
> analysis. Memory analysis not only helps solve this situation but also 
> provides unique insights in the runtime of the systemâ€™s activity: open 
> network connections, recently executed commands, and the ability to see 
> any decrypted malicious file. There are plenty of traces of someone's 
> activity on a computer, but perhaps some of the most valuble information 
> can be found within memory dumps, that is images taken of RAM. These dumps 
> of data are often very large, but can be analyzed using a tool called 
> Volatility provided by the Volatility Foundation.**
> 
> **Volatility is an open-source memory forensics framework for incident 
> response and malware analysis. This is a very powerful tool and we can 
> complete lots of interactions with memory dump files, such as:**
> 
> - **List all processes that were running.**
> - **List active and closed network connections.**
> - **View internet history.**
> - **Identify files on the system and retrieve them from the memory dump.**
> - **Read the contents of notepad documents.**
> - **Retrieve commands entered into the Windows Command Prompt.**
> - **Scan for the presence of malware using YARA rules.**
> - **Retrieve screenshots and clipboard contents.**
> - **Retrieve hashed passwords.**
> - **Retrieve SSL keys and certificates.**
> 
> > - SCIST.Entry.forensics.txt

> ![upload_c7bdda3e79e1b74fe70eff8fa15e8ba1](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/2be7cf6c-8f96-43a9-8bad-00ff6b51f64f)
> 
> > - SCIST_address

> There's also a hint
> ![upload_6f74d6604ef609c67705e571d0b249a4](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/f11b731e-164a-492f-a070-a0ae4be35d51)
>
> > - [root] -> C:\
5. As you follow the hint, you should use your Mem Forensics skills/tools to solve **.MEM file** hidden challenge; but first, let us find hints / stories in Directory that mentioned in `Malicious_Image.ad1.txt`:
    ```
    - C:\Users\sixkwnp\Contacts
    - C:\Users\sixkwnp\Videos
    ```
- First story that later will use in solving final challenge

**C:\Users\sixkwnp\Videos:**
> ![upload_f3002fa1e3c3facafbea798688d4c9db](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/47c338af-4524-4e0d-9915-aa8397e6040d)

> [17:35] <phy114c1<3r>:
> --
> **I have some news for you. I found a way to hack into the confidential computer of the company we are targeting.**
> 
> [17:36] <phy114c1<3r>: 
> --
> **it's not easy, but it's possible. You need to use a special software that I developed, and a code word that I will give you.**
> 
> [17:37] <phy114c1<3r>:
> --
> **are you interested?**
> 
> [17:40] \<zal0>:
> --
> **of course I am! This is what we have been waiting for. How did you do it?**
> 
> [17:41] <phy114c1<3r>:
> --
> **I can't tell you the details here, it's too risky. But I can show you the software and the code word when we meet.**
> 
> [17:42] <phy114c1<3r>:
> --
> **the software is called Zephyr, and it can bypass the security system of the computer. The code word is ZÌ´Í†Ì†Ì¾Ì†Í„Í„Ì„ÍŠÍ„Í‚ÌƒÍÌ¾Ì£Í–Ì¦Ì²Ì­Ì™Ì­ÌÌ©Ì¦ÌÌ—Í‰aÌ´Í›ÌÌÌ¿ÌšÌ¾Í€Ì…ÌšÌ“Ì‘ÌÍ‚ÌšÌ’Ì€ÍÌ–ÌŸÌžÌžÌ¬Ì¦Í™Ì°Ì±lÒˆÍÌÍŠÍÍÌ€Ì™ÍˆÌœÍˆÍ…Ì˜gÌ¶ÌŽÌ„Í—Ì‰Ì‘Ì¦Í–Ì°Ì¤Ì–Ì¬Ì™Ì©Ì¦oÌ´ÍÌ“ÌÍŒÌˆÌ€Í‹Ì€Í—Í—Í†Í‚Ì«ÌŸÌ®Ì²Ì­Í™Ì°ÌžÍ‰Í‰.Ì¸Í›Í‹ÌˆÌšÍÌ‘ÌŽÌ‡ÌÌŠÌÌÍ€Í—Ì¾Í‚Í“ÌÍ•Ì±ÍÌ Í•Ì¤ÍˆÍ™Ì¬ÌŸ.Ì¶Ì‚Í’ÌŒÍŒÍ€Í’Ì„ÌªÍÌ±Í™Ì¥ÍˆÌžÌ®Ì©Í…Í“Ì˜ÍŽÍÌœ.Ì¶ÍŠÍ€Ì¿ÍŒÌ¾Ì½ÌÌ†Ì¾Í“ÍˆÌ¬Í–Ì£Ì³Ì±Í•Í‡ÌªÌ³Ì™Ì˜Í•, just put the software into the confidential computer in ur company.**
> 
> [17:43] \<zal0>:
> --
> **Zephyr and ZÌ´Í†Ì†Ì¾Ì†Í„Í„Ì„ÍŠÍ„Í‚ÌƒÍÌ¾Ì£Í–Ì¦Ì²Ì­Ì™Ì­ÌÌ©Ì¦ÌÌ—Í‰aÌ´Í›ÌÌÌ¿ÌšÌ¾Í€Ì…ÌšÌ“Ì‘ÌÍ‚ÌšÌ’Ì€ÍÌ–ÌŸÌžÌžÌ¬Ì¦Í™Ì°Ì±lÒˆÍÌÍŠÍÍÌ€Ì™ÍˆÌœÍˆÍ…Ì˜gÌ¶ÌŽÌ„Í—Ì‰Ì‘Ì¦Í–Ì°Ì¤Ì–Ì¬Ì™Ì©Ì¦oÌ´ÍÌ“ÌÍŒÌˆÌ€Í‹Ì€Í—Í—Í†Í‚Ì«ÌŸÌ®Ì²Ì­Í™Ì°ÌžÍ‰Í‰.Ì¸Í›Í‹ÌˆÌšÍÌ‘ÌŽÌ‡ÌÌŠÌÌÍ€Í—Ì¾Í‚Í“ÌÍ•Ì±ÍÌ Í•Ì¤ÍˆÍ™Ì¬ÌŸ.Ì¶Ì‚Í’ÌŒÍŒÍ€Í’Ì„ÌªÍÌ±Í™Ì¥ÍˆÌžÌ®Ì©Í…Í“Ì˜ÍŽÍÌœ.Ì¶ÍŠÍ€Ì¿ÍŒÌ¾Ì½ÌÌ†Ì¾Í“ÍˆÌ¬Í–Ì£Ì³Ì±Í•Í‡ÌªÌ³Ì™Ì˜Í•. Got it. When can we meet?**
>
> **ã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸã„ŽÌ¸Ì”Ì‰ÌƒÍ‹ÌÍ€Ì€Ì…Í•ÍÌÌ¤Ì²Ì±ÌÌœÌªÌ«Ì£Ì¥Ì™Í“ã„•ÌµÌŽÌ„ÌÌšÍŽÍŽÌ™Ì­ÌÌ¥Í™Í•Ì­Ì¥Ì®Ì˜ÍŽã„ŠÌµÌŠÌ’Ì„ÌÍ€Ì‰ÌŠÌ†ÌšÍ‡ÌÌ«ÍÍŽÍˆÌ­Ì©ÌŸ**
> 
> [17:45] <phy114c1<3r>:
> --
> **tomorrow night, at the usual place. Be careful, and don't tell anyone else about this.**
> 
> [17:46] \<zal0>:
> --
> **don't worry, I won't. See you tomorrow, hacker buddy.**
> 
> [17:47] <phy114c1<3r>:
> --
> **see you tomorrow.**
> 
> > - confidential.cam
6. Check other dirs
```
C:\Users\sixkwnp\Contacts
```
> C:\Users\sixkwnp\Contacts:
> 
> ![upload_e6904e10460cc8a583b2718b6a83ff4b](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/3e4031c4-484d-4e06-ad01-074c074161e1)
> 
---
- **Hint twice about the location of .MEM file challenge** below
> C:\Users\sixkwnp\Contacts\Company Confidential:
> 
> ![upload_7053ba3289dfcc9f04506504146b712c](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/ad1ec946-a69d-4d23-a1b1-bd3c7879561d)
> 
---
- Second story that later will use in solving final challenge
    
**C:\Users\sixkwnp\Contacts\Andrew:**
> ![upload_da6625ba65d3dc284764a8b55bcb7961](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/d4f1184f-abd0-403f-a6de-549e9897b4c9)


> **\<Zoe> 2023-07-06 16:46:00**
> --
> **I'm ready. Today is my last day at this company. Are you sure you've inserted the malicious software into the highly confidential computer?**
> 
> **\<Andrew> 2023-07-06 16:46:15**
> --
> **Don't worry, I've taken care of everything. Once you press the send key, it will trigger the malicious software and crash the entire system.**
> 
> **\<Zoe> 2023-07-06 16:46:30**
> --
> **That's great. I've been waiting for this day for a long time. This company and CEO Mattias deserve to be punished for all the unfair things they've done to us.**
> 
> **\<Andrew> 2023-07-06 16:46:45**
> --
> **Yeah, they never value our work and contributions. They only exploit our labor and intelligence. We need to show them that we're not to be messed with.**
> 
> **\<Zoe> 2023-07-06 16:47:00**
> --
> **Let's take action together then. I'm pressing the send key now. Goodbye, Andrew. I hope you find a better job.**
> 
> **\<Andrew> 2023-07-06 16:47:15**
> --
> **Goodbye, Zoe. I wish you all the best. We may never see each other again, but I'll always remember you.**
> 
> > - Andrew\1.txt
---
- **Hint third about .MEM challenge and some tools + challenges**

**C:\Users\sixkwnp\Contacts\Zoe || C:\Users\sixkwnp\Contacts\Zephyr:**
> ![upload_3a85a555b353dbefd7476142a7c19d0a](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/23cb52b7-fb1a-4eaf-8f70-2a91568fb6a8)

> **Browsing History**
> 
> **https://en.wikipedia.org/wiki/Memory_forensics**
> 
> **8.8.8.8**
> 
> **https://github.com/apsdehal/awesome-ctf**
> 
> **https://github.com/volatilityfoundation/volatility3**
> > - Zoe\1.txt && Zephyr\1.txt
7. And then we can go to **`C:\Windows\SysWOW64\Recovery\Company\SCIST.fixed.mem`** to solve Mem Forensics chanllenge.
- Use build-in funtion (FTK imager, Autopsy, other tools...) to **Export the .MEM file**
> ![upload_0b18795407a4a9f445a3737684ebeb69](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/fb46721c-c48e-4dbd-ac9b-14e9b3d35b41)
    - ![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/9afa88a3-d1c6-4184-912e-c098534a5427)
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
Just use `volatility2` or other tools to find **Powershell/CMD history**, we can easily find the hacker had tried below in the terminal.
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
> ![upload_0397d79ec10ada2d45057e30897bb0f0](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/90e5f786-5f0d-4a97-a190-b902a3503deb)
> 
> > - C:\Users\sixkwnp\Appdata\Local\Temp
10. If you take a glance at two stories and `C:\Users\sixkwnp\Contacts`,
easily can understand who is `zal0`, `phy114ck3r` and `CEO`, respectively.
---
> ![upload_0c7e1ce0744d5054d5115482ae4a78fb](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/1e854f1b-5b78-42b8-9b9d-6afde55f361b)
> 
> - **key: Zoe (Case sensitive)**
> 
> > - Who is zal0 CASESENSITIVE.7z
---
> ![upload_ae7cbb4467f4cb26732ea6b16ae152fb](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/63b9e991-d14f-4c38-b678-ba4d529fd45c)
> 
> - **key: Andrew (Case sensitive)**
> 
> > - Who is phy114ck3r.7z
---
> ![upload_08018ee34a18c1db1e1e1f4cf109d66f](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/cc20febd-1ef4-42a8-ac12-9f3452e84c1c)
> 
> - **key: Mattias (Case sensitive)**
> 
> > - Who is CEO.7z
11. Successfully dump the flag!
    
![](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/06447641-4c0f-431c-aef6-9112fd8f2dc5)
- > ![upload_21520a484f556fb87738faf8891b562d](https://github.com/sixkwnp/SCIST-S3-Final-CTF-Misc/assets/67849251/b46f01b1-ede9-4188-9514-86b35114616a)
- > > 112f3a99b283a4e1788dedd8e0e5d35375c33747.png
### - **Final FLAG : SCIST{Vol4T1L17Y_C4N_do_4LL_Non53nse_M3MFoR3Ns1cs}**

