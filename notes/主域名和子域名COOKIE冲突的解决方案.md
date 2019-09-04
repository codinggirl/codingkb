---
tags: [GitHub]
title: 主域名和子域名COOKIE冲突的解决方案
created: '2019-09-04T08:24:32.125Z'
modified: '2019-09-04T08:24:35.289Z'
---

##  主域名和子域名COOKIE冲突的解决方案

之前有用户反馈在某些情况下，登录AKCMS提示成功，但是紧接着又要求登录，陷入死循环。之前调试过数次，看到的现象是写入COOKIE后，紧接着刷新页面读出来COOKIE就和刚才写的不同，一直都没找到原因。

今天偶然的机会找到原因了，原来是COOKIE的一个缺陷：

写入COOKIE的时候可以指定域名，但“读”的时候不可以。写入可以指定域名意味着为abc.akcms.com域名写一个COOKIE（test=1），再为akcms.com域名写入一个COOKIE（test=2）都可以成功写入。但PHP“读”的时候却只能读出一个叫test的COOKIE，他的值到底是1还是2呢？结论很荒唐：不知道。

根据写入顺序的不同、浏览器的不同，什么情况都有，有的是1有的是2。

为什么PHP“读”COOKIE的“读”要加一个引号呢？因为PHP是无法直接读浏览器的COOKIE的，只能被动接受浏览器发起的HTTP请求，而HTTP请求的头部可以插入COOKIE内容，插入的是什么，PHP读到的就是什么，PHP没得选择。浏览器组织COOKIE的时候，不但读取当前域名下的COOKIE也读取它的根域名的COOKIE，当COOKIE的键冲突时（比如上面说的test），浏览器以自己的一套逻辑选一个提交给HTTP服务器（Apache、Nginx等），然后Apache们再把关于php的请求通过CGI之类的东西转发给PHP解析器，这才能在PHP中读出COOKIE。

浏览器舍弃冲突COOKIE的逻辑到底是什么呢？据有的同志亲测：有的是无条件子域名COOKIE优先（safari），有的则是按先后顺序取舍，没有一套统一的标准。这就导致我们在开发程序时比较为难，一旦一个优先级高的根域名的同名COOKIE存在，那么你PHP程序不管怎么设置COOKIE也白搭。

解决办法：封装COOKIE的写入读取、避免出现COOKIE重名的可能性。以下是AKCMS中关于COOKIE的实现，供大家参考。

function aksetcookie($name, $value, $expire = 0, $fore = 0) {
	global $cookiepre, $currenturl;
	$domain = $\_SERVER\['HTTP\_HOST'\];
	if($fore == 0) {
		$key = md5($domain.'\_'.$cookiepre.'\_'.$name.'\_cp');
	} else {
		$key = md5($domain.'\_'.$cookiepre.'\_'.$name.'\_fore');
	}
	return setcookie($key, $value, $expire);
}

function akgetcookie($name, $fore = 0) {
	global $cookiepre, $currenturl;
	$domain = $\_SERVER\['HTTP\_HOST'\];
	if($fore == 0) {
		$key = md5($domain.'\_'.$cookiepre.'\_'.$name.'\_cp');
	} else {
		$key = md5($domain.'\_'.$cookiepre.'\_'.$name.'\_fore');
	}
	if(isset($\_COOKIE\[$key\])) {
		return $\_COOKIE\[$key\];
	} else {
		return false;
	}
}

##### 本文链接地址：[http://www.yubosun.com/tech/domain\-cookie\-conflict.htm](http://www.yubosun.com/tech/domain-cookie-conflict.htm)

##### 标签: [PHP](http://www.yubosun.com/tag.php?keywords=PHP)  [cookie](http://www.yubosun.com/tag.php?keywords=cookie)

