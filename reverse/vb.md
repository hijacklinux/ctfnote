---
layout: default
---
# ![](../img/hj.jpg)逆向工程-PC逆向-VB
## ![](../img/hj.jpg)VB破解关键
### 针对变量
```
__vbaVarTstEq 常用
__vbaVarTstNe
__vbaVarCompEq 常用
__vabVarCompLe
__vabVarCompLt
__vabVarCompGe
__vabVarCompGt
__vabVarCompNe
```
### 针对字符串
```
__vbaStrCmp 常用
__vbaStrComp
__vbaStrCompVar
__vbaStrLike
__vbaStrTextComp
__vbaStrTextLike
```
## ![](../img/hj.jpg)《VB P-code的调试方法总结》
>这篇文章是我曾经在看雪论坛发布的，这次弄了网站，也放上来吧

P-code伪编码，用od太麻烦，需用到WKTVBDebugger
### 方法1
```
把cm放到wktv目录下面，打开，运行
机器码与命令：
BranchF:     机器码1C  类似jnz/jne   如果堆栈为0就跳
BranchT:     机器码1D  类似je/jz       如果堆栈为-1就跳
Branch:       机器码1E  类似jmp        无条件跳
```
单击‘高级信息’或‘Analize BranchX' 可看到当前进程所有跳转位置
```
EqVarBool:  机器码33   比较指令，根据结果将0或-1压入堆栈
ConcatStr:  机器码2A    字符串连接指令 ，此指令单步跟踪时会在日志窗口留下相应结果，可ctr+O在此处下断
LitI2_Byte:   机器码F4    将数据压入堆栈
FLdZeroAd/CVarStr:取字符串指令，特点同ConcatStr
```
### 方法2
```
也可结合VB Decompiler找到if地址，（或直接在这里推导算法），小贱个人喜欢用这种方法直接推导算法
去od的dump窗口找这个地址，把1C改成1D即可爆破
```
### 方法3
```
VBParser先分析一下，
然后倒入od,在关键处下内存断点，单步跟
```
>因为小贱喜欢用方法2,另外两种嘛，不是说不好，只是...我太懒~
>现在用方法2举个栗子


```
cm导入vb decompiler:
Dim var_11C As Variant
  Dim var_176 As Integer
  loc_40E28C: If (Me.txtname.Text = vbNullString) Then
  loc_40E2B0:   MsgBox("You have to enter you name first.", &H40, "Error", var_FC, var_11C)
  loc_40E2C0:   Exit Sub
  loc_40E2C1: End If
  loc_40E2E1: If (Me.txtkey.Text = vbNullString) Then
  loc_40E305:   MsgBox("You have to enter a key first.", &H40, "Error", var_FC, var_11C)
  loc_40E315:   Exit Sub
  loc_40E316: End If
  loc_40E336: If (Me.txtkey.Text = vbNullString) Then
  loc_40E35A:   MsgBox("You have to enter at least 5 chars.", &H40, "Error", var_FC, var_11C)
  loc_40E36A:   Exit Sub

 loc_40E36B: End If
                                                                                                       上面是查看name和key是否为空且name必须长度大于5
  loc_40E393: For var_14C = 1 To CVar(Len(Me.txtname.Text)): var_12C = var_14C 'Variant                变量12c=变量14c
  loc_40E3C1:   var_FC = Mid(CVar(Me.txtname.Text), CLng(var_12C), 1)                                  FC轮询name每一位

 loc_40E3D5:   var_11C = var_94 &
CVar(Asc(CStr(var_FC)))
                                                                                                        变量94= name每一位变体，并组合，例如name为‘11111’那么变量94=‘4949494949’
  loc_40E3D9:   var_94 = var_11C 'Variant
  loc_40E3EF: Next var_14C 'Variant
  loc_40E3F5: ' Referenced from: 40E422

 loc_40E404: If (Len(var_94) > 9)
Then
                                                                                                         变量94反复除以3.14直到变量94为9位数
  loc_40E41E:   var_94 = Fix((var_94 / 3.141592654)) 'Variant                                            并赋值给变量94
  loc_40E422:   GoTo loc_40E3F5
  loc_40E425: End If

 loc_40E449: var_94 = (var_94 Xor &H30F85678 - CVar(global_76))
'Variant
                                                                                                          变量94与30F85678异或，再减去全局变量76，通过其他函数可以查到全局变量，大家自己动手查查
  loc_40E45A: For var_170 = 1 To 10: var_12C = var_170 'Variant
  loc_40E489:   If (Me.txtkey.Text = global_52(CLng(var_12C))) Then
  loc_40E48C:   End If
  loc_40E48F: Next var_170 'Variant
  loc_40E4DE: If ((CVar(Me.txtkey.Text) - var_94) = CVar(Len(Me.txtname.Text))) Then                      密码-变量94=name长度，得密码
  loc_40E502:   MsgBox("Wow, you have found a correct key!", &H40, "Correct key", var_FC, var_11C)
  loc_40E533:   MsgBox("Mail me, how you got it: CyberBlade@gmx.net ", &H40, "Correct key!", var_FC, var_11C)
  loc_40E550:   Me.Command2.Caption = "Exit"
  loc_40E55B: Else
  loc_40E567:   global_80 = (global_80 + 1)
  loc_40E570:   var_176 = global_80
  loc_40E579:   If (var_176 = 6) Then
  loc_40E5B3:     If (MsgBox("-=Do you need a hint ?=-", &H24, "I can't stand it anymore", var_FC, var_11C) = 7) Then
  loc_40E5B6:       Exit Sub
  loc_40E5BA:     Else
  loc_40E5DB:       MsgBox("Forget it.", &H40, "he, he...", var_FC, var_11C)
  loc_40E5F0:       global_80 = 0
  loc_40E5F3:     End If
  loc_40E5F6:   Else
  loc_40E5FC:     If (var_176 > 3) Then

 loc_40E620:       MsgBox("Have you ever been trying to be successful in
 cracking my password ?", &H20, "Failed", var_FC, var_11C)
  loc_40E633:     Else
  loc_40E639:       If (var_176 <= 3) Then
  loc_40E65D:         MsgBox("Sorry, wrong key.", &H40, "Failed", var_FC, var_11C)
  loc_40E66D:       End If
  loc_40E66D:     End If
  loc_40E66D:   End If
  loc_40E66D: End If
  loc_40E677: Me.txtkey.SetFocus
  loc_40E67F: Exit Sub
  ```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
