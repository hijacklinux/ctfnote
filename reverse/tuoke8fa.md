---
layout: default
---
# ![](../img/hj.jpg)PC逆向-C,C++,mfc,delphi,易语言-OD
## 《关于徒手脱壳的几种方法》
>这篇文章是我曾经在看雪论坛发布的，这次弄了网站，也放上来吧,当然方法是各路大牛前辈发明的

#### 首先我们先来说说壳的原理吧，简单说下就好，带壳程序运行以后，都会做哪些事情呢？ （想要了解更多的朋友们就去读看雪大哥的《加密与解密（第三版）》吧）
>1、保存现场(pushad/popad,pushfd/popfd)
>
>2、获取壳自己需要的API地址
>
>3、解密原程序各个区块
>
>4、IAT的初始化
>
>5、重定位
>
>6、Hook-API
>
>7、跳到 OEP

#### 首先我们先用DIE来查一下带壳程序是什么语言编写的，然后再用OD载入。
#### 在脱壳之前呢，我们一定要知道各个语言的OEP特征是什么，免得到时候就算到了OEP，自己都不知道，那可就悲剧了，这里列出各个语言的OEP（请忽略地址）：
#### ![](../img/github3.png)VC++入口
```
00496EB8 >/$  55            PUSH EBP
00496EB9  |.  8BEC          MOV EBP,ESP
00496EBB  |.  6A FF         PUSH -1
00496EBD  |.  68 40375600   PUSH Screensh.00563740
00496EC2  |.  68 8CC74900   PUSH Screensh.0049C78C
00496EC7  |.  64:A1 0000000>MOV EAX,DWORD PTR FS:[0]
00496ECD  |.  50            PUSH EAX
00496ECE  |.  64:8925 00000>MOV DWORD PTR FS:[0],ESP
00496ED5  |.  83EC 58       SUB ESP,58
```
#### ![](../img/github4.png)VB入口
```

00401166  - FF25 6C104000   JMP DWORD PTR DS:[<&MSVBVM60.#100>]      ; MSVBVM60.ThunRTMain
0040116C >  68 147C4000     PUSH PACKME.00407C14                     ；这是第一种入口格式
00401171    E8 F0FFFFFF     CALL <JMP.&MSVBVM60.#100>
00401176    0000            ADD BYTE PTR DS:[EAX],AL
00401178    0000            ADD BYTE PTR DS:[EAX],AL
0040117A    0000            ADD BYTE PTR DS:[EAX],AL
0040117C    3000            XOR BYTE PTR DS:[EAX],AL

00401FBC >  68 D0D44000        push dumped_.0040D4D0
00401FC1    E8 EEFFFFFF        call <jmp.&msvbvm60.ThunRTMain>        ；这是第二种入口格式
00401FC6    0000               add byte ptr ds:[eax],al
00401FC8    0000               add byte ptr ds:[eax],al
00401FCA    0000               add byte ptr ds:[eax],al
00401FCC    3000               xor byte ptr ds:[eax],al
00401FCE    0000               add byte ptr ds:[eax],al
```
#### ![](../img/github5.png)BC++入口
```
0040163C > $ /EB 10         JMP SHORT BCLOCK.0040164E
0040163E     |66            DB 66                                    ;  CHAR 'f'
0040163F     |62            DB 62                                    ;  CHAR 'b'
00401640     |3A            DB 3A                                    ;  CHAR ':'
00401641     |43            DB 43                                    ;  CHAR 'C'
00401642     |2B            DB 2B                                    ;  CHAR '+'
00401643     |2B            DB 2B                                    ;  CHAR '+'
00401644     |48            DB 48                                    ;  CHAR 'H'
00401645     |4F            DB 4F                                    ;  CHAR 'O'
00401646     |4F            DB 4F                                    ;  CHAR 'O'
00401647     |4B            DB 4B                                    ;  CHAR 'K'
00401648     |90            NOP
00401649     |E9            DB E9
0040164A   . |98E04E00      DD OFFSET BCLOCK.___CPPdebugHook
0040164E   > \A1 8BE04E00   MOV EAX,DWORD PTR DS:[4EE08B]
00401653   .  C1E0 02       SHL EAX,2
00401656   .  A3 8FE04E00   MOV DWORD PTR DS:[4EE08F],EAX
0040165B   .  52            PUSH EDX
0040165C   .  6A 00         PUSH 0                                   ; /pModule = NULL
0040165E   .  E8 DFBC0E00   CALL <JMP.&KERNEL32.GetModuleHandleA>    ; \GetModuleHandleA
00401663   .  8BD0          MOV EDX,EAX
```
#### ![](../img/github6.png)Delphi入口
```
00509CB0 > $  55            PUSH EBP
00509CB1   .  8BEC          MOV EBP,ESP
00509CB3   .  83C4 EC       ADD ESP,-14
00509CB6   .  53            PUSH EBX
00509CB7   .  56            PUSH ESI
00509CB8   .  57            PUSH EDI
00509CB9   .  33C0          XOR EAX,EAX
00509CBB   .  8945 EC       MOV DWORD PTR SS:[EBP-14],EAX
00509CBE   .  B8 20975000   MOV EAX,unpack.00509720
00509CC3   .  E8 84CCEFFF   CALL unpack.0040694C
```
#### ![](../img/github7.png)易语言入口
```

格式一：
00401000 >  E8 06000000     call dump_.0040100B
00401005    50              push eax
00401006    E8 BB010000     call <jmp.&KERNEL32.ExitProcess>
0040100B    55              push ebp
0040100C    8BEC            mov ebp,esp
0040100E    81C4 F0FEFFFF   add esp,-110
00401014    E9 83000000     jmp dump_.0040109C
00401019    6B72 6E 6C      imul esi,dword ptr ds:[edx+6E],6C
0040101D    6E              outs dx,byte ptr es:[edi]

格式二
00403831 >/$  55            PUSH EBP
00403832  |.  8BEC          MOV EBP,ESP
00403834  |.  6A FF         PUSH -1
00403836  |.  68 F0624000   PUSH Nisy521.004062F0
0040383B  |.  68 A44C4000   PUSH Nisy521.00404CA4
00403840  |.  64:A1 0000000>MOV EAX,DWORD PTR FS:[0]
00403846  |.  50            PUSH EAX
00403847  |.  64:8925 00000>MOV DWORD PTR FS:[0],ESP

5.0以后独立编译的入口：

0045C12D >/$  55            push ebp
0045C12E  |.  8BEC          mov ebp,esp
0045C130  |.  6A FF         push -0x1
0045C132  |.  68 087D4B00   push notebook.004B7D08
0045C137  |.  68 14E94500   push notebook.0045E914
0045C13C  |.  64:A1 0000000>mov eax,dword ptr fs:[0]
0045C142  |.  50            push eax
0045C143  |.  64:8925 00000>mov dword ptr fs:[0],esp
0045C14A  |.  83EC 58       sub esp,0x58
0045C14D  |.  53            push ebx
0045C14E  |.  56            push esi
0045C14F  |.  57            push edi
0045C150  |.  8965 E8       mov [local.6],esp
0045C153  |.  FF15 A4C14700 call dword ptr ds:[<&KERNEL32.GetVersion>;  kernel32.GetVersion
0045C159  |.  33D2          xor edx,edx
0045C15B  |.  8AD4          mov dl,ah
0045C15D  |.  8915 BCB44E00 mov dword ptr ds:[0x4EB4BC],edx
0045C163  |.  8BC8          mov ecx,eax
0045C165  |.  81E1 FF000000 and ecx,0xFF
0045C16B  |.  890D B8B44E00 mov dword ptr ds:[0x4EB4B8],ecx
0045C171  |.  C1E1 08       shl ecx,0x8
0045C174  |.  03CA          add ecx,edx
0045C176  |.  890D B4B44E00 mov dword ptr ds:[0x4EB4B4],ecx
0045C17C  |.  C1E8 10       shr eax,0x10
0045C17F  |.  A3 B0B44E00   mov dword ptr ds:[0x4EB4B0],eax
0045C184  |.  6A 01         push 0x1
0045C186  |.  E8 354B0000   call notebook.00460CC0
```
#### ![](../img/github8.png)MASM32 / TASM32入口
```
00401258 >/$  6A 00         push 0                                   ; /pModule = NULL
0040125A  |.  E8 47000000   call <jmp.&kernel32.GetModuleHandleA>    ; \GetModuleHandleA
0040125F  |.  A3 00304000   mov dword ptr ds:[403000],eax
00401264  |.  6A 00         push 0                                   ; /lParam = NULL
00401266  |.  68 DF104000   push dump.004010DF                       ; |DlgProc = dump.004010DF
0040126B  |.  6A 00         push 0                                   ; |hOwner = NULL
0040126D  |.  6A 65         push 65                                  ; |pTemplate = 65
0040126F  |.  FF35 00304000 push dword ptr ds:[403000]               ; |hInst = NULL
00401275  |.  E8 56000000   call <jmp.&user32.DialogBoxParamA>       ; \DialogBoxParamA
```
#### ![](../img/github9.png)VC8入口
```
00401258 >/$  6A 00         push 0                                   ; /pModule = NULL
0040125A  |.  E8 47000000   call <jmp.&kernel32.GetModuleHandleA>    ; \GetModuleHandleA
0040125F  |.  A3 00304000   mov dword ptr ds:[403000],eax
00401264  |.  6A 00         push 0                                   ; /lParam = NULL
00401266  |.  68 DF104000   push dump.004010DF                       ; |DlgProc = dump.004010DF
0040126B  |.  6A 00         push 0                                   ; |hOwner = NULL
0040126D  |.  6A 65         push 65                                  ; |pTemplate = 65
0040126F  |.  FF35 00304000 push dword ptr ds:[403000]               ; |hInst = NULL
00401275  |.  E8 56000000   call <jmp.&user32.DialogBoxParamA>       ; \DialogBoxParamA
```
### ![](../img/hj.jpg)正题——脱壳方法
#### ![](../img/github10.png)脱壳方法一：单步跟踪，其实就是f8,f7配合啦
>小贱提示：需要注意的几点：当你遇到近Call的时候需要用f7跟进；当你遇到远Call的时候，用f8跳过就行；当遇到循环的时候，直接用f4跳出；当遇到大的跳转就要注意了，很快就到OEP了
>
>我们拿《加密与解密》的RebPE.exe举栗子，die查VC++写的，来看看吧

```
00413000 >  60              pushad                                   ; 刚进来
00413001    E8 C2000000     call RebPE.004130C8                      ; 这call很近吧，我们f7进去
================================================================================================进来了
004130C8    5D              pop ebp                                  ; RebPE.00413006
004130C9    81ED 06000000   sub ebp,0x6                              ; 进来以后就f8跟吧
004130CF    8B85 C0000000   mov eax,dword ptr ss:[ebp+0xC0]
004130D5    0BC0            or eax,eax
=================================================================================================用我单身狗多年练就的手速开始疯狂f8
00150028    8B06            mov eax,dword ptr ds:[esi]               ; kernel32.GetProcAddress
0015002A    8907            mov dword ptr ds:[edi],eax               ; kernel32.GetProcAddress
0015002C    83C6 04         add esi,0x4
0015002F    83C7 04         add edi,0x4
00150032  ^ E2 F4           loopd short 00150028                     ; 看到这里，这是个循环，我们直接下一条选中f4跳出循环
00150034    8D85 37010000   lea eax,dword ptr ss:[ebp+0x137]
0015003A    8982 55030000   mov dword ptr ds:[edx+0x355],eax         ; kernel32.GetProcAddress
=================================================================================================
001500DE    68 00800000     push 0x8000
001500E3    6A 00           push 0x0
001500E5    56              push esi
001500E6    FF95 5D030000   call dword ptr ss:[ebp+0x35D]            ; kernel32.VirtualFree
001500EC    5B              pop ebx                                  ; ntdll.7C930460
001500ED    83C3 0C         add ebx,0xC
001500F0  ^ EB B3           jmp short 001500A5
001500F2    8B85 8D020000   mov eax,dword ptr ss:[ebp+0x28D]         ; 上面jmp又是个向上跳转，在这条f4跳出
001500F8    0BC0            or eax,eax
=================================================================================================
00150270    8B85 89020000   mov eax,dword ptr ss:[ebp+0x289]
00150276    0385 51030000   add eax,dword ptr ss:[ebp+0x351]         ; RebPE.00400000
0015027C    0185 84020000   add dword ptr ss:[ebp+0x284],eax         ; RebPE.00401130
00150282    61              popad                                    ; 注意这里有个popad，看到他就离OEP不远了，因为popad是还原现场用的
00150283    68 00000000     push 0x0
00150288    C3              retn
================================================================================================
00401130      55            db 55                                    ;  来到了这里，用ctrl+A分析下
00401131      8B            db 8B
00401132      EC            db EC
00401133      6A            db 6A                                    ;  CHAR 'j'
00401134      FF            db FF
00401135      68            db 68                                    ;  CHAR 'h'
================================================================================================分析好了，对比下上面的入口，正好就是OEP了
00401130  /.  55            push ebp                                 ;  来到了这里，用ctrl-A分析下
00401131  |.  8BEC          mov ebp,esp
00401133  |.  6A FF         push -0x1
00401135  |.  68 B8504000   push RebPE.004050B8
0040113A  |.  68 FC1D4000   push RebPE.00401DFC                      ;  SE 处理程序安装
0040113F  |.  64:A1 0000000>mov eax,dword ptr fs:[0]
```
#### ![](../img/github11.png)脱壳方法二：最后一次异常法
```
1、在od的调试选项把异常的那个对话框所有异常的勾选全部去掉，ctrl+f2重新载入
2、反复按shift+f9,记录你按下的次数n
3、重新载入，这次按n-1次shift+f9
4、看堆栈，有个se处理程序，反汇编跟随
5、单步跟踪，直到有大跳转
```
#### ![](../img/github12.png)脱壳方法三：两次断点法（内存镜像法）——外壳会先解压各个区段，然后再跳回代码段执行，根据这个原理
```
1、在od的调试选项把异常的那个对话框所有异常的勾选全部选中
2、alt+m查看内存，然后再在资源段下一次f2断点，f9断下（目的是断下后确定壳解压处理了text段）
3、接下来再在text段下一次f2断点，f9断下（此时就说明开始访问text段了）
4、单步跟踪，直到大跳转
```
#### ![](../img/github13.png)脱壳方法四：ESP定律——外壳在开始的时候一定要保存环境（例如pushad）,结束的时候还原环境（例如popad），最重要的是堆栈一定要平衡，这样，当执行pushad后，我们就可以在ESP下断点
```

EAX 00000000                                              ；这是执行pushad后各个寄存器的值
ECX 0012FFB0
EDX 7C92E514 ntdll.KiFastSystemCallRet
EBX 7FFD5000
ESP 0012FFA4                                              ；我们在0012fff0处下个硬件断点，然后运行
EBP 0012FFF0
ESI 7C9A0620 ntdll.7C9A0620
EDI 7C930460 ntdll.7C930460
EIP 00413001 RebPE.00413001
================================================================================================
00150283    68 30114000     push 0x401130                 ；断在这里了，我们单步跟踪
00150288    C3              retn
00150289    3011            xor byte ptr ds:[ecx],dl
0015028B    0000            add byte ptr ds:[eax],al
0015028D    0100            add dword ptr ds:[eax],eax
================================================================================================到达OEP
00401130    55              push ebp
00401131    8BEC            mov ebp,esp
00401133    6A FF           push -0x1
00401135    68 B8504000     push RebPE.004050B8
0040113A    68 FC1D4000     push RebPE.00401DFC
0040113F    64:A1 00000000  mov eax,dword ptr fs:[0]
00401145    50              push eax
```
#### ![](../img/github14.png)脱壳方法五：直接搜索popad法
```
这方法原理特别简单，既然popad是还原环境，那么直接搜索ctrl+f搜索popad，然后下f2断点，运行断下就好了。
当然，这种方法有很大的局限性，只适合UPX，ASPACK等少量壳。
```
#### ![](../img/github15.png)脱壳方法六：模拟跟踪法
```
tc的意思：Trace in till condition 跟踪进入直到条件满足
eip是当前指令指针

1、在od的调试选项把异常的那个对话框所有异常的勾选全部选中
2、alt+m查看内存，然后再在资源段下一次f2断点，f9断下（目的是断下后确定壳解压处理了text段）
3、接下来再在text段下一次f2断点，f9断下（此时就说明开始访问text段了）
================================================================================================ 以上方法就是两次断点，不同的是下面不再单步跟踪了，而是模拟跟踪
alt+m去看下内存，里面在程序领空会有一个sfx 输入表，像这样：
00400000   00001000   RebPE                 PE 文件头
00401000   00004000   RebPE      .text      代码
00405000   00001000   RebPE      .rdata     数据
00406000   00003000   RebPE      .data
00409000   0000A000   RebPE      .rsrc      资源
00413000   00007000   RebPE      .pediy     SFX,输入表            ；我们记住这个地址00413000
================================================================================================
之后我们在command命令行插件中输入tc eip<00413000 回车，看到左上角状态变成了‘跟踪’，接下来就是等程序自动跳到OEP了
```
#### ![](../img/github16.png)脱壳方法七：sfx法，这方法就是太慢了
```

1、在od的调试选项把异常的那个对话框所有异常的勾选全部选中
2、在od的调试选项在SFX的那个选项卡选中 “字节方式跟踪真正入口处（速度非常慢）”
3、重新载入，漫长的等待过后就到达OEP了
=================================================================================================等待过后
00401130  /.  55            push ebp                                 ;  SFX 代码真正入口点
00401131  |.  8BEC          mov ebp,esp
00401133  |.  6A FF         push -0x1
00401135  |.  68 B8504000   push RebPE.004050B8
0040113A  |.  68 FC1D4000   push RebPE.00401DFC                      ;  SE 处理程序安装
0040113F  |.  64:A1 0000000>mov eax,dword ptr fs:[0]
```
#### ![](../img/github17.png)脱壳方法八：一步到达法
```

1、od载入后运行
2、我们去堆栈的最下面，开始向上找，找第一个程序领空地址的
=================================================================================================
0012FFA4  |FFFFFFFF
0012FFA8  |0012FF4C
0012FFAC  |0012FFF0
0012FFB0  |0012FFE0  指向下一个 SEH 记录的指针
0012FFB4  |00401DFC  SE处理程序
0012FFB8  |004050B8  RebPE.004050B8                          ；就是这个，RebPE是程序名，注意左边有个框把这段给框起来了，说明这是一个函数堆栈
0012FFBC  |00000000
0012FFC0  \0012FFF0
0012FFC4   7C816037  返回到 kernel32.7C816037
0012FFC8   7C930460  返回到 ntdll.7C930460
0012FFCC   7C9A0620  ntdll.7C9A0620
=================================================================================================我们继续向上翻，找这个框的开头

0012FF14  |00000000
0012FF18  |7FFDE000
          \
向下找，‘返回到’的，这个例子有3个
0012FF1C  /0012FFC0                                                      ;这个就是函数的头部了
0012FF20  |0040101B  返回到 RebPE.0040101B 来自 user32.DialogBoxParamA    ；这个肯定不是，user32.DialogBoxParamA
0012FF24  |00400000  RebPE.00400000
0012FF28  |00000065
0012FF2C  |00000000
0012FF30  |00401080  RebPE.00401080
0012FF38  |004011FE  返回到 RebPE.004011FE 来自 RebPE.00401000            ；排除第一个和第三个，也只剩下这个了，我们右键反汇编跟随
0012FF3C  |00400000  RebPE.00400000
0012FF40  |00000000
0012FF44  |002823C8
0012FF48  |0000000A
0012FF4C  |7C930460  返回到 ntdll.7C930460                                ；这个也肯定不是，都跑系统领空去了
0012FF50  |7C9A0620  ntdll.7C9A0620
0012FF54  |7FFDD000
=================================================================================================反汇编跟随后，向上找断首，就是OEP了
00401130  /.  55            push ebp
00401131  |.  8BEC          mov ebp,esp
00401133  |.  6A FF         push -0x1
00401135  |.  68 B8504000   push RebPE.004050B8
0040113A  |.  68 FC1D4000   push RebPE.00401DFC                      ;  SE 处理程序安装
0040113F  |.  64:A1 0000000>mov eax,dword ptr fs:[0]
00401145  |.  50            push eax
00401146  |.  64:8925 00000>mov dword ptr fs:[0],esp
0040114D  |.  83EC 58       sub esp,0x58
00401150  |.  53            push eb
```
### 以上就是我掌握的徒手脱壳的方法了，不是每个方法都试用所有的壳，怎么说呢，挨个试试吧。当然也可以脱壳机或者用一堆脚本，不过这篇文章的主题是‘徒手’脱壳，就列出这几种方法，如果有什么不对的，还请指正

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
