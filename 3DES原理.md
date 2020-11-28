# 3DES原理

相当于对每个数据应用3次DES加密的对称加密算法。

它使用2-3个56位的密钥对数据进行三次加密。但相对于AES，处理速度较慢，安全性也有欠缺。

## 3DES加密过程

假设E(), D()分别为DES的加密和解密函数，P为明文，C为密文，加解密公式为：

**加密**：
$$
C = E_{K3}\left(D_{K2}\left(E_{K1}\left(P\right)\right)\right)
$$
**解密**：
$$
P = D_{K1}\left(E_{K2}\left(D_{K3}\left(C\right)\right)\right)
$$
这里的K3，K2，K1是3个8Bytes的密钥，若三者不相同，实际上是用了一个168位的密钥进行加密。因为单次DES加密是56位。

## 补充说明：

NoPadding：不对数据进行处理

ZerosPadding：无数据字节填充0

PKCS5Padding：对数据除8，得余数m，对后面m位补充m个8。解密时，从最后一个字节删除m个8

PKCS7Padding：加密块字节数不同，原理与PKCS5Padding相同









