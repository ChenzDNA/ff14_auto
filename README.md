## 最终幻想14自动弹奏

1. ### [前言](#preface)
2. ### [键位设置](#keyset)
3. ### [开始](#start)
4. ### [曲谱格式](#format)
5. ### [后续更新](#update)

---

1. ### <p id='preface'>前言</p>

主程序是ff14_auto.exe，txt文件是曲谱文件，程序通过读取曲谱文件，根据曲谱文件里的内容建立链表，再模拟成键盘信号输出，就能实现游戏里自动弹琴。

其实这个版本是修改了几次之后的版本。

~~之前的版本 代码太丢人了 写的跟坨屎样~~

**程序需要管理员权限，请右键-属性-兼容性-勾选 以管理员身份运行此程序-确定**

1. ### <p id='keyset'>键位设置</p>

游戏中的键位设置，从左到右：

黑键键位：<kbd>1</kbd> <kbd>2</kbd> <kbd>3</kbd> <kbd>4</kbd> <kbd>5</kbd> <kbd>6</kbd> <kbd>7</kbd> <kbd>8</kbd> <kbd>
9</kbd> <kbd>0</kbd> <kbd>K</kbd> <kbd>L</kbd> <kbd>O</kbd> <kbd>P</kbd> <kbd>,</kbd>

白键键位：<kbd>Z</kbd> <kbd>X</kbd> <kbd>C</kbd> <kbd>V</kbd> <kbd>B</kbd> <kbd>N</kbd> <kbd>M</kbd> <kbd>A</kbd> <kbd>
S</kbd> <kbd>D</kbd> <kbd>F</kbd> <kbd>G</kbd> <kbd>H</kbd> <kbd>J</kbd> <kbd>Q</kbd> <kbd>W</kbd> <kbd>E</kbd> <kbd>
R</kbd> <kbd>T</kbd> <kbd>Y</kbd> <kbd>U</kbd> <kbd>I</kbd>

目前程序不支持自定义键位，必须在最终幻想14中设置成这种键位。

3. ### <p id='start'>开始</p>

打开程序，若曲谱文件和程序在同一个文件夹下，则直接输入曲谱名就行（注意要带后缀），若不在同一个文件夹下，输入绝对位置或相对位置均可。

若输入文件名有误，则会提示` cannot find the file: [文件名]`，重新输入即可。

程序打开文件成功后，会提示`----切换到最终幻想14游戏中开始弹奏----`，这时程序会每隔100ms检测当前激活的窗口是否为最终幻想14，当切换到最终幻想14窗口后，程序会自动开始键盘信号的模拟。

4. ### <P id='format'>曲谱格式</p>

* 第一行：调号 (C,D,E,F,G,A,B) ，'#'和'b'作为升号和降号。例`D#`
* 第二行：BPM (曲速)，2~3位的数字
* 第三行：音符。

#### 音符的格式：

* 首字符是数字 (表示不同的音高)，同样能用'#'和'b'紧跟首字符表示升号和降号。
* 用'.'和'`'表示低音点和高音点，写在音高之后。
* 用'\_'和'-'表示时值减半或加一拍，通过复数的'_'或'-'可以构成全音符、二分音符等不同时值的音符。 ( 注：不能混用 )

  具体如下：( X表示音高 )
    * 全音符：X---
    * 二分音符：X-
    * 四分音符：X
    * 八分音符：X_
    * 十六分音符：X__  &nbsp;( 2* '_' )
    * 三十二分音符：X___ &nbsp;( 3* '_' )
* 用':'表示附点音符，要写在音符的最后面。
* 每个音符用一个空格隔开，当需要改变调号和BPM的时候，音符末尾要有空格，另起一行写调号，后一行写BPM。**调号和BPM不能单独改变。** 再后一行写音符。
* 文件最后的音符后面不要有空格。
* **写的时候不要开中文输入法，会造成符号错误。**

  例：
    ```
    C
    120
    1 2 3 1 1 2 3 1 
    D
    120
    3 4 5 3 4 5
    ```

5. ### <p id='update'>后续更新</p>

* #### 2021.2.24
  更新了同时按下多个按键的功能。

  例：
  ```
  1.23.4`56.7`---
  ```
  对于连在一起的音符，程序会以 20ms 为间隔依次触发按键，只有最后一个音符后的时间是有效时间。
* #### 2021.5.28
  把弹奏的音域限制在 ff14 的音域内。如果超过 ff14 乐器的音域，就会把整个曲子降到 ff14 的音域内，如果低于 ff14 的音域就直接忽略（音调变成0）
* #### 2021.6.19
  之前是用的 Sleep() 来设置延时，但是频繁的调用 Sleep() 会因为分配 CPU 时间从而造成性能问题。所以用了一个比较笨的无限循环判断当前的毫秒数，达到设定的毫秒数就退出循环 ~~装睡~~。
  <br>
  <br>
  实测 Sleep(10) 消耗时间 16ms 左右，循环消耗 11ms 左右。（i5-9300H，16G RAM，GTX1650，512GB SN720）
  <br>
  <br>
  当然这种替代方法有缺点，CPU 占用达到了 20% 之多。之前使用 Sleep() 时 CPU 占用基本上看不出来。不过电脑不好应该不会影响这个方法的准确度，毕竟循环的时间准确度取决于每次循环的间隔时间。
<hr>
ps：图标也是我画的 :D

B站个人空间：https://space.bilibili.com/37864003 里面有介绍视频。