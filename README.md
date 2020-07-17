# Music-Player

## Music-Player v2.0

+ ### 什么是Music Player

**Music Player**是由**Timothy Liu**制作的音乐播放器，或**Timothy Music Player Language**（以下简称**TMPL**）解释器，可以在Windows系统上播放由TMPL编写的音乐。  

+ ### 如何使用Music Player  

使用Music Player的方式有两种，一种是直接打开，另一种是通过cmd打开：

1. 直接打开  
   直接打开Music Player（名称为mpv.exe），将会有3个选项，选择A，可以手动输入由TMPL编写的文件的路径，Music Player将自动识别并播放TMPL所编写的音乐。  

2. 通过cmd打开  
   使用cmd打开Music Player（mpv.exe），参数可以有两种：TMPL文件路径和内置指令。若参数为文件路径，Music Player将自动识别并播放TMPL所编写的音乐；内置指令有两种：`-v`和`-o`，若为`-v`则会显示Music Player的版本信息，若为`-o`则会播放Music Player的内置主音乐，例如下面的指令将会先显示版本信息，再播放src.txt中的TMPL代码所编写的音乐，再播放内置主音乐：  

    ```cmd
        mpv -v src.txt -o
    ```

3. 拖动打开（不是说两种吗）  
   将含有TMPL的文件拖动到mpv.exe上即可播放该文件中TMPL编写的音乐（实际上是第二种方式的简单操作方式）

+ ### 关于Timothy Music Player Language（TMPL）  

TMPL是一种基于简谱的语言，Music Player解释该语言时会自动忽略无效字符。具体语法如下：  

1. 表示音符
   TMPL用阿拉伯数字1\~7分别表示do\~xi，用`^`前缀表示高音，`_`前缀表示低音，例如`^1`表示高音do，`_6`表示低音la，而阿拉伯数字0则表示休止符  

2. 表示音符长短
   单独的一个音符表示一个四分音符，音符后加一个短线`-`表示一个二分音符，加三个短线表示一个全音符，例如`2-`表示一个二分音符re，而`^2---`表示一个全音符高音re。  
   音符被`[]`括住表示一个八分音符，被`{}`括住表示一个十六分音符，例如：`[5]`表示一个八分音符so，`{_5}`表示一个十六分音符低音so。
   音符后加`.`表示附点（延长到原来的1.5倍），例如`{_4}.`、`7.`等。  

实例：  

《两只老虎》  

```TMPL
    12311231345-345-[5][6][5][4]31[5][6][5][4]311_51-1_51-
```  

---

《小燕子》  

```TMPL
    [3][5][^1][6]5-[3][5][6][^1]5-
    ^1.[^3]^2^1[^2][^1][6][^1]5-
    3.[5]6[5][6]^1[^2][5]6-
    [3][2]12-
    2[2][3]55^1[2][3]5-
    [3][5][^1][6]5-
    [3][5][6][^1]5-
    ^1.[^3]^2^1[^2][^1][6][^1]5-
    3.[5]6[5][6]^1[^2][5]6-
    3.[^1]65[3][2]12-
    2.[3]5-^1.[^3]^2^1[^2][^1][5][6]^1-
```

---
---

## Music Player v3.0 更新内容  

---

### 1. TMPL语法提升

#### (1) 增加升音

+ **语法增强**：在音符的前面加上`#`前缀表示升音  

+ **举例**：例如`#4`代表升fa、`#_5`代表升低音so  

#### (2) 可以表示更短的音符  

+ **语法改变**：`[]`和`{}`不再分别表示特定的音符，而改为分别使其内的音符长度缩短至原来的0.5和0.25倍，且可以嵌套  

+ **举例**：例如可以用`{[5]}`表示三十二分音符so、用`{34}`或`[[34]]`表示原来的`{3}{4}`、用`[55[5]]`表示原来的`[5][5]{5}`  

+ **说明**：语法改变是向前兼容的，例如`[5]`和`{5}`仍然分别表示八分音符和十六分音符so；但`[]`、`{}`与`-`之间是不可嵌套的，例如`[5-]`、`{5-}`等都是不正确的  

### 2. 软件功能增强  

+ 可以在播放音乐的同时打印出正在播放的音符


实例：  

《两只老虎》  

用新的TMPL语言可以改为：  

```TMPL
    12311231345-345-[5654]31[5654]311_51-1_51-
```  

---

《小燕子》  

用TMPL表示为：  

```TMPL
    [35^16]5-[356^1]5-
    ^1.[^3]^2^1[^2^16^1]5-
    3.[5]6[56]^1[^25]6-
    [32]12-
    2[23]55^1[23]5-
    [35^16]5-
    [356^1]5-
    ^1.[^3]^2^1[^2^16^1]5-
    3.[5]6[56]^1[^25]6-
    3.[^1]65[32]12-
    2.[3]5-^1.[^3]^2^1[^2^156]^1-
```

---
---

## Music Player v3.1 更新内容  

---

### TMPL语法提升

开头三行代表乐曲的本身属性：

+ 第一行仅一个**大写字符**，代表大调，如第一行写一个字符`C`，表示C大调，但目前没有实质性作用，设置的目的参见下文。

+ 第二行是一个分数y/x，代表以x分音符为一拍，每小节有y拍。y没有实质性作用，但不可缺少，另外字符`/`也是不可缺少的。

+ **特别注意（兼容性提醒）：为了保持一定的向前兼容性和有利于后面的继续开发，整个文件的第一个代表大调的字母用来标识自TMPL3.1以后的文件，即开头非大写字母的旧式文件仍然在v3.1中兼容。但若旧式文件的开头是大写字母，则不能不是v3.1及以后版本中合法的文件。**

实例：  

《两只老虎》  

在新的TMPL语言中为：  

```TMPL
C
2/4
75
12311231345-345-[5654]31[5654]311_51-1_51-
```  

---

《小燕子》  

用新的TMPL表示为：  

```TMPL
C
4/4
75
[35^16]5-[356^1]5-
^1.[^3]^2^1[^2^16^1]5-
3.[5]6[56]^1[^25]6-
[32]12-
2[23]55^1[23]5-
[35^16]5-
[356^1]5-
^1.[^3]^2^1[^2^16^1]5-
3.[5]6[56]^1[^25]6-
3.[^1]65[32]12-
2.[3]5-^1.[^3]^2^1[^2^156]^1-
```
