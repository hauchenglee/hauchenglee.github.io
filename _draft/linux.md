---
layout: post
title: Linux
category: tech
tags: [tech]
---

## Basic

一般來說著名的linux系統基本上分兩大類：

- RedHat系列：RedHat、CentOs、Fedora等
- Debian系列：Debian、Ubuntu等

## BIOS

<table>
    <thead>
        <tr>
            <th></th>
            <th>old</th>
            <th>new</th>
            <th>linux</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>開機檢測程式</td>
            <td>BIOS</td>
            <td>UTFI (Unified Extensible Fimware Interface)</td>
            <td></td>
        </tr>
        <tr>
            <td>磁碟分割表</td>
            <td>MBR (Master Boot Record)</td>
            <td>GPT (GUID Partition Table)</td>
            <td></td>
        </tr>
        <tr>
            <td>開機管理程式</td>
            <td>boot loader</td>
            <td>boot loader</td>
            <td>grub</td>
        </tr>
    </tbody>
</table>

## 安裝

-> 鳥哥建議

<table>
    <thead>
        <tr>
            <th>mount point</th>
            <th>desired capacity</th>
            <th>device type</th>
            <th>size policy</th>
            <th>size policy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>biosboot</td>
            <td>2M</td>
            <td>standerd partition</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>/boot</td>
            <td>1G</td>
            <td>standerd partition</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>/</td>
            <td>10G</td>
            <td>LVM</td>
            <td>fixed</td>
            <td>30G</td>
        </tr>
        <tr>
            <td>/home</td>
            <td>5G</td>
            <td>LVM</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>swap</td>
            <td>1G</td>
            <td>LVM</td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

## 權限

<table>
    <thead>
        <tr>
            <th></th>
            <th>r</th>
            <th>w</th>
            <th>x</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>可讀取此一文件的實際內容</td>
            <td>可以編輯、新增或是修改該文件的內容</td>
            <td>該文件具有可以被系統執行的權限</td>
        </tr>
        <tr>
            <td>d</td>
            <td>讀取目錄結構清單的權限，若不具有權限，無法使用tab補齊文件名</td>
            <td>
            1. 建立新的文件與目錄<br>
            2. 刪除文件目錄，無論權限為何<br>
            3. 將文件或目錄進行更名<br>
            4. 搬移目錄內文件、目錄位置
            </td>
            <td>進入該目錄的權限（key）</td>
        </tr>
    </tbody>
</table>

<br>

<table>
    <thead>
        <tr>
            <th>操作動作</th>
            <th>/dir1</th>
            <th>/dir1/file</th>
            <th>/dir2</th>
            <th>重點</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>讀取file1內容</td>
            <td>x</td>
            <td>r</td>
            <td>-</td>
            <td>要能夠進入/dir1才能讀到裡面的文件資料</td>
        </tr>
        <tr>
            <td>修改file1內容</td>
            <td>x</td>
            <td>rw</td>
            <td>-</td>
            <td>能夠進入/dir1且修改file1才行</td>
        </tr>
        <tr>
            <td>執行file1內容</td>
            <td>x</td>
            <td>rx</td>
            <td>-</td>
            <td>能夠進入/dir1且file1能夠運作才行</td>
        </tr>
        <tr>
            <td>刪除file1</td>
            <td>wx</td>
            <td>-</td>
            <td>-</td>
            <td>能夠進入/dir1具有目錄修改的權限即可</td>
        </tr>
        <tr>
            <td>將file1複製到/dir2</td>
            <td>x</td>
            <td>r</td>
            <td>wx</td>
            <td>要能夠讀file1且能夠修改/dir2內的資料</td>
        </tr>
    </tbody>
</table>

- 要能新增、查詢、修改、複製文件，當前目錄最低限度權限為`x`，對方目錄為`wx`
   - 文件讀取：r
   - 文件修改：rw
   - 文件執行：rx
   - 複製原始文件：r
- 要能刪除文件當前目錄最低限度權限為`wx`
   - 文件刪除：-
- 預設權限，使用者文件或目錄預設值權限 - 想要扣除的權限

## 遠端

- ms remote desktop
- check point
- WinScp -> win - linux
- putty -> remote control / ssh client
- nginx

## 目錄

dir|name|desc
---|---|---
~|root|/root
|user|/home/username
/bin|binary|机器只能跑二进制语言  所以里面放置的是一些可执行的文件
/boot|boot|這個目錄主要在放置開機會使用到的檔案，包括Linux核心檔案以及開機選單與開機所需設定檔等等。
/dev|device|任何装置与接口设备都是以文件的型态存在于这个目录当中的
/etc|etceteras|“附加项目、零星杂物”  ，系統主要的設定檔幾乎都放置在這個目錄內，例如人員的帳號密碼檔、 各種服務的啟始檔等等。一般來說，這個目錄下的各檔案屬性是可以讓一般使用者查閱的， 但是只有root有權力修改。
/lib|library|系统的函数库，结合编程来说就有点像是存放头文件的地方。
/media|media|放置的就是可移除的装置，如软盘，DVD都暂挂载在这里
/mnt|mount|“挂载”的意思，用于 暂时挂载某些额外的装置
/opt|optional application software package|加了一些理解过后的翻译是 “第三方软件目录”  即是存放第三方软件的目录，比如你安装个wine QQ啥的就放在这个里面
/run|run|早期的 FHS 規定系統開機後所產生的各項資訊應該要放置到 /var/run 目錄下，新版的 FHS 則規範到 /run 底下。 由於 /run 可以使用記憶體來模擬，因此效能上會好很多！
/sibn|system binary|比起binary在前面加了system，所以里面存放的是   开机过程中所需要的指令
/srv|service|存放的 “是一些网络服务启动之后，这些服务所需要取用的数据目录”
/tmp|temp|"暂时的,临时的" 这是让一般使用者或者是正在执行的程序暂时放置文件的地方
/usr|Unix Software Resource|"Unix操作系统软件资源"   暂时不太清楚里面放的什么  鸟哥书上类比说类似于windows的 “C:\Windows\ + C:\Program files”
/var|variables|"可变的"。引用鸟哥的话：“如果/usr是安装时会占用较大硬盘容量的目录，那么/var就是在系统运作后才会渐渐占用硬盘容量的目录。因为/var目录主要针对常态性变动的文件，包括缓存(cache)、登录档(log file)以及某些软件运作所产生的文件，包括程序文件(lock file, run file)，或者例如MySQL数据库的文件等等”<br><br>因此这个文件夹的占用内存会随着电脑的使用慢慢 “变大”， 对应了  "variables" !!!
/home|home|这个不用多说，点进去看看里面有什么文件夹就知道了
/lib64|lib<qual>|用來存放與 /lib 不同的格式的二進位函式庫，例如支援 64 位元的 /lib64 函式庫等
/root|root|系統管理員(root)的家目錄。之所以放在這裡，是因為如果進入單人維護模式而僅掛載根目錄時， 該目錄就能夠擁有root的家目錄，所以我們會希望root的家目錄與根目錄放置在同一個分割槽中。

Ref: [Linux中的全称与简称(不断更新)_qw4990的专栏-CSDN博客](https://blog.csdn.net/qw4990/article/details/11534523){:target="_blank"}

## Reference

- [鳥哥的 Linux 私房菜 -- 鳥哥的 Linux 私房菜 首頁](http://linux.vbird.org/){:target="_blank"}

---