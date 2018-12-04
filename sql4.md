######其他类型注入的详解(3)

###3.CBC字节反转攻击

####知识点:AES的CBC模式加密
#####加密过程:
>IV:用于随机化加密的字符串</br>
			ciphertext_0=Encrypt(Plaintext XOR IV)只用于第一组块</br>
			ciphertext_n=Encrypt(Plaintext XOR Ciphertext_N-1)用于除第一组之外的模块

![1.png](https://i.loli.net/2018/09/12/5b98dc5c44de5.jpg)

***
#####解密过程:
>plaintext_0=Decrpt(Ciphertext) XOR IV 只用于第一组块</br>
	plaintext_n=Decrpt(Ciphertext) XOR Ciphertext_n-1

![2.png](https://i.loli.net/2018/09/12/5b98dc5c63805.jpg)

***

####CBC攻击

**攻击发生在解密过程中,实质上就是通过更改上一块的内容,来间接修改明文中的内容,你在密文中改变的字节,只会影响到在下一明文当中,具有相同偏移量的字节.因此,进行cbc攻击仅影响到两个块.
主要用途:体现在不知道加密密钥的情况下,通过修改密文,可以间接修改明文**

![3.png](https://i.loli.net/2018/09/12/5b98dd24bba15.jpg)

####实际应用:Bugku-login4
[login4](http://123.206.31.85:49168/)