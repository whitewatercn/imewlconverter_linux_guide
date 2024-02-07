
近期我在使用rime输入法时遇到一个问题：找不到好用的医学类词库，此时可以通过”[深蓝词库转换](https://github.com/studyzy/imewlconverter)”将搜狗细胞词库转换为rime可以用的`XXX.dict.yaml`文件

深蓝最常用在Windows系统上，相关教程很多，但很少有人提到Linux上怎么用（Linux用户应该能够自己琢磨，大概是这样吧），本文是一篇教程，以Google colab为例（基于Ubuntu）

你可以直接在colab里使用这些代码 
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Hw72igOAxvqL3JFU_DYvnQg9pLKnim7O?usp=sharing)



> ⚠️注意⚠️：以下操作命令行前面的`!`仅用于colab的notebook环境，如果是Linux终端，不需要`!`
# 下载深蓝软件并解压
请下载最新版本
```
! wget https://github.com/studyzy/imewlconverter/releases/download/v3.0.0/imewlconverter-v3.0.0-macOS.zip
! unzip imewlconverter-v3.0.0-macOS.zip
```



# 下载dotnet6.0
深蓝只能用6.0启动
```
! apt-get install dotnet-sdk-6.0
```

# 启动深蓝，获取帮助
```
! dotnet ImeWlConverterCmd.dll -?
```
你将得到以下输出
```
xpy	手心输入法
xlpy	新浪拼音
jd	极点五笔
jdzm	极点郑码
jdmb	极点五笔.mb文件
xywb	小鸭五笔
yahoo	雅虎奇摩
ld2	灵格斯ld2
wb86	五笔86版
wb98	五笔98版
wbnewage	五笔新世纪版
cjpt	仓颉平台
emoji	Emoji
bdsj	百度手机或Mac版百度拼音
bdsje	百度手机英文
bcd	百度手机词库bcd
qqsj	QQ手机
ifly	讯飞输入法
self	自定义
word	无拼音纯汉字

例如要将./test.scel和./a.scel的搜狗细胞词库转换为./gg.txt的谷歌拼音词库，命令为：
dotnet ImeWlConverterCmd.dll -i:scel ./test.scel ./a.scel -o:ggpy ./gg.txt
例如要将./test.scel和./a.scel的搜狗细胞词库转换为./temp文件夹下的谷歌拼音词库test.txt和a.txt，命令为：
dotnet ImeWlConverterCmd.dll -i:scel ./test.scel ./a.scel -o:ggpy ./temp/*
例如要将./test/*.scel的搜狗细胞词库转换为./temp文件夹下的谷歌拼音词库，命令为：
dotnet ImeWlConverterCmd.dll -i:scel ./test/*.scel -o:ggpy ./temp/*

对于导入词库不包含词频，而导出时需要指定词频，可以通过-r:命令指定词频的生成方式，支持的有：
-r:baidu  根据该词语在百度搜索的结果数量决定词频
-r:google  根据该词语在Google搜索的结果数量决定词频(需翻墙)
-r:数字  指定一个固定数字的词频

对于导出词库为Rime输入法的，可以通过-ct:pinyin/wubi/zhengma设置编码，也可通过-os:windows/macos/linux设置适用的操作系统

使用-ft:可以设置词条的过滤条件，如果不设置则不过滤任何词条。-ft:后面可以设置的过滤条件包括：
len:1-100 保留字数为1到100的词条
rank:2-9999 保留词频在2到9999的词条
rm:eng 移除包含英文字母的词条
rm:num 移除包含数字的词条
rm:space 移除包含空格的词条
rm:pun 移除包含标点符号的词条
以上过滤条件可以组合，同时起作用，用竖线分开即可：
-ft:"len:1-100|rank:2-9999|rm:eng|rm:num|rm:space|rm:pun"

自定义格式的参数如下:
-f:213,|byyn
213 这里是设置拼音、汉字和词频的顺序，213表示1汉字2拼音3词频，必须要有3个
, 这里是设置拼音之间的分隔符，用逗号分割
| 这里是设置汉字拼音词频之间的分隔符，用|分割
b 这里是设置拼音分隔符的位置，有lrbn四个选项，l表示左包含，r表示右包含，b表示两边都包含，n表示两边都不包含
yyn 这里是设置拼音汉字词频这3个是否显示，y表示显示，b表示不显示，这里yyn表示显示拼音和汉字，不显示词频
例如要将一个qpyd词库转换为自定义格式的文本词库，拼音之间逗号分割，拼音和词之间空格分割，不显示词频，同时使用自定义的编码文件code.txt命令如下：
dotnet ImeWlConverterCmd.dll -i:qpyd ./a.qpyd -o:self ./zy.txt "-f:213, nyyn" -c:./code.txt
其中-c:./code.txt指定的编码文件格式为：“汉字<Tab键>编码”每行一个。

最后，如果这款软件帮助到了您，您可以通过捐赠表示感谢，捐赠作者支付宝地址：studyzy@163.com 曾毅
输入 -? 可获取帮助
```

# 上传原词库文件后，启动转换

更多使用参数见`! dotnet ImeWlConverterCmd.dll -?`
```
! dotnet ImeWlConverterCmd.dll -i:scel ./acupuncture.scel -o:rime ./acupuncture.txt
```

这样你就得到了`acupuncture.txt`，里面的内容就是我们需要的

# 最后
我已经将一些搜狗医学词库转换成rime格式，欢迎下载使用
https://github.com/whitewatercn/rime_clinic
