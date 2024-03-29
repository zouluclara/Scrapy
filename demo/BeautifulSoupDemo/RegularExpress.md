#### 正则表达式

> 'PY'开头,后续存在不多于10个字符,后续字符不能是'P'或'Y' <==> 正则表达式：PY[^PY]{0,10}

##### 常用操作符

|操作符 |说明 |实例|
|-------|----|----|
|. |表示任何单个字符||
|[ ] |字符集，对单个字符给出取值范围 |[abc]表示a、 b、 c， [a‐z]表示a到z单个字符|
[^ ] |非字符集，对单个字符给出排除范围 |[^abc]表示非a或b或c的单个字符|
|* |前一个字符0次或无限次扩展 |abc* 表示 ab、 abc、 abcc、 abccc等|
|+ |前一个字符1次或无限次扩展 |abc+ 表示 abc、 abcc、 abccc等|
|? |前一个字符0次或1次扩展 |abc? 表示 ab、 abc|
|\| |左右表达式任意一个 | abc\|def 表示 abc、 def|
|{m} |扩展前一个字符m次 |ab{2}c表示abbc|
|{m,n} |扩展前一个字符m至n次（含n） |ab{1,2}c表示abc、 abbc|
|^ |匹配字符串开头 |^abc表示abc且在一个字符串的开头|
|$ |匹配字符串结尾 |abc$表示abc且在一个字符串的结尾|
|( ) |分组标记，内部只能使用 \| 操作符 |(abc)表示abc， (abc|def)表示abc、 def|
|\d |数字，等价于[0‐9]||
|\w |单词字符，等价于[A‐Za‐z0‐9_]||

##### 正则表达式实例
|正则表达式 | 对应字符串|
|----------|----------|
|P(Y|YT|YTH|YTHO)?N |'PN'、 'PYN'、 'PYTN'、 'PYTHN'、 'PYTHON'|
|PYTHON+ |'PYTHON'、 'PYTHONN'、 'PYTHONNN' …|
|PY[TH]ON |'PYTON'、 'PYHON'|
|PY[^TH]?ON |'PYON'、 'PYaON'、 'PYbON'、 'PYcON'…|
|PY{:3}N |'PN'、 'PYN'、 'PYYN'、 'PYYYN'…|

##### 经典正则表达式实例

|正则表达式 | 对应字符串|
|----------|----------|
|^[A‐Za‐z]+$|由26个字母组成的字符串|
|^[A‐Za‐z0‐9]+$|整数形式的字符串|
|^‐?\d+$|整数形式的字符串|
|^[0‐9]*[1‐9][0‐9]*$|正整数形式的字符串|
|[1‐9]\d{5}|中国境内邮政编码， 6位|
|[\u4e00‐\u9fa5]|匹配中文字符|
|\d{3}‐\d{8}\|\d{4}‐\d{7}|国内电话号码， 010‐68913536|

##### 匹配ip地址的正则表达式


* IP地址字符串形式的正则表达式（ IP地址分4段，每段0‐255）:
	* \d+.\d+.\d+.\d+ 或 \d{1,3}.\d{1,3}.\d{1,3}.\d{1,3}

* 精确写法 
	* 0‐99： [1‐9]?\d
	* 100‐199: 1\d{2}
	* 200‐249: 2[0‐4]\d
	* 250‐255: 25[0‐5]
* 完整
	* (([1‐9]?\d|1\d{2}|2[0‐4]\d|25[0‐5]).){3}([1‐9]?\d|1\d{2}|2[0‐4]\d|25[0‐5])

#### Re lib

* raw string类型（原生字符串类型）
* re库采用raw string类型表示正则表达式，表示为：r'text'
* raw string是不包含对转义符再次转义的字符串
* 建议：当正则表达式包含转义符时，使用raw string

> eg: r'\d{3}‐\d{8}|\d{4}‐\d{7}'

* re库也可以采用string类型表示正则表达式，但更繁琐

> eg: '\\d{3}‐\\d{8}|\\d{4}‐\\d{7}'

##### Re lib 的主要功能

|函数 |说明|
|-----|---|
|re.search(pattern, string, flags=0) |在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象|
|re.match(pattern, string, flags=0) |从一个字符串的开始位置起匹配正则表达式，返回match对象|
|re.findall() |搜索字符串，以列表类型返回全部能匹配的子串|
|re.split()| 将一个字符串按照正则表达式匹配结果进行分割，返回列表类型|
|re.finditer() |搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象|
|re.sub() |在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串|

* pattern : 正则表达式的字符串或原生字符串表示
* string : 待匹配字符串
* flags : 正则表达式使用时的控制标记
* maxsplit: 最大分割数，剩余部分作为最后一个元素输出
* repl : 替换匹配字符串的字符串
* count : 匹配的最大替换次数

###### flags

|常用标记 |说明|
|--------|----|
|re.I re.IGNORECASE |忽略正则表达式的大小写， [A‐Z]能够匹配小写字符|
|re.M re.MULTILINE |正则表达式中的^操作符能够将给定字符串的每行当作匹配开始|
|re.S re.DOTALL |正则表达式中的.操作符能够匹配所有字符，默认匹配除换行外的所有字符|

##### Re lib的另一种等价方法

```python
rst = re.search(r'[1‐9]\d{5}', 'BIT 100081')
>>> pat = re.compile(r'[1‐9]\d{5}')
>>> rst = pat.search('BIT 100081')
```

> 面向对象用法：编译后的多次操作,将正则表达式的字符串形式编译成正则表达式对象

```python
regex = re.compile(pattern, flags=0)
>>> regex = re.compile(r'[1‐9]\d{5}')
```

##### Re lib的Match对象

* Match对象是一次匹配的结果，包含匹配的很多信息

```python
>>> match = re.search(r'[1‐9]\d{5}', 'BIT 100081')
>>> if match:
print(match.group(0))
>>> type(match)
<class '_sre.SRE_Match'>
```

###### Match对象的属性

|属性 |说明|
|-----|----|
|.string |待匹配的文本|
|.re |匹配时使用的patter对象（正则表达式）|
|.pos |正则表达式搜索文本的开始位置|
|.endpos |正则表达式搜索文本的结束位置|

###### Match对象的方法

|方法 |说明|
|-----|----|
|.group(0) |获得匹配后的字符串|
|.start() |匹配字符串在原始字符串的开始位置|
|.end() |匹配字符串在原始字符串的结束位置|
|.span() |返回(.start(), .end())|

#### Re lib的贪婪匹配和最小匹配

* Re库默认采用贪婪匹配，即输出匹配最长的子串

```python
>>> match = re.search(r'PY.*N', 'PYANBNCNDN')
>>> match.group(0)
'PYANBNCNDN'
```

如何输出最短的子串呢？

```python
>>> match = re.search(r'PY.*?N', 'PYANBNCNDN')
>>> match.group(0)
'PYAN'
```

###### 最小匹配操作符

* 只要长度输出可能不同的，都可以通过在操作符后增加?变成最小匹配

|操作符 |说明|
|------|----|
|*? |前一个字符0次或无限次扩展，最小匹配|
|+? |前一个字符1次或无限次扩展，最小匹配|
|?? |前一个字符0次或1次扩展，最小匹配|
|{m,n}? |扩展前一个字符m至n次（含n），最小匹配|