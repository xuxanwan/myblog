---
layout: post
title:  "hex 编码"
date:   2017-07-2
categories: java
---
## 在编码的时候如何将byte[]数据以String保存
我了解到的办法是: base64和hex.这次来了解下hex.

### hex是什么:
1. hex的全称为hexadecimal Strings.也就是16进制字符串.
2. 也就是说hex是通过换算,将原先的byte转换成16进制字符串.所以hex的源字符串为0~9 和A~F. 

### 为什么要用hex:
1.  hex提供了简洁的bit描述方法. 使用16进制时,一个byte可以通过00~FF来完全涵括.

### 怎么用hex:
根据apache工具类里(commons-codec-1.9.jar)对Hex的的具体实现.转成hex的思路还是很简单的,对2进制的每4位分别映射到字符表中.
```java
/**
     * Converts an array of bytes into an array of characters representing the hexadecimal values of each byte in order.
     * The returned array will be double the length of the passed array, as it takes two characters to represent any
     * given byte.
     *
     * @param data
     *            a byte[] to convert to Hex characters
     * @param toDigits
     *            the output alphabet
     * @return A char[] containing hexadecimal characters
     * @since 1.4
     */
    protected static char[] encodeHex(final byte[] data, final char[] toDigits) {
        final int l = data.length;
        final char[] out = new char[l << 1];
        // two characters form the hex value.
        for (int i = 0, j = 0; i < l; i++) {
            out[j++] = toDigits[(0xF0 & data[i]) >>> 4];
            out[j++] = toDigits[0x0F & data[i]];
        }
        return out;
    }
```
反转回来的操作如下:
```java
    /**
     * Converts an array of characters representing hexadecimal values into an array of bytes of those same values. The
     * returned array will be half the length of the passed array, as it takes two characters to represent any given
     * byte. An exception is thrown if the passed char array has an odd number of elements.
     *
     * @param data
     *            An array of characters containing hexadecimal digits
     * @return A byte array containing binary data decoded from the supplied char array.
     * @throws DecoderException
     *             Thrown if an odd number or illegal of characters is supplied
     */
    public static byte[] decodeHex(final char[] data) throws DecoderException {
        final int len = data.length;
        if ((len & 0x01) != 0) {
            throw new DecoderException("Odd number of characters.");
        }
        final byte[] out = new byte[len >> 1];
        // two characters form the hex value.
        for (int i = 0, j = 0; j < len; i++) {
            int f = toDigit(data[j], j) << 4;
            j++;
            f = f | toDigit(data[j], j);
            j++;
            out[i] = (byte) (f & 0xFF);
        }
        return out;
    }
```

### 思考
- 将源数据转换成byte,并用hex编码后长度变化会是这么样:
 如果源数据是UTF-8格式的数据,根据定义:
   >  128个US-ASCII字符只需一个字节编码（Unicode范围由U+0000至U+007F）。
   >  带有附加符号的拉丁文、希腊文、西里尔字母、亚美尼亚语、希伯来文、阿拉伯文、叙利亚文及它拿字母则需要两个字节编码（Unicode范围由U+0080至U+07FF）。
   >  其他基本多文种平面（BMP）中的字符（这包含了大部分常用字，如大部分的汉字）使用三个字节编码（Unicode范围由U+0800至U+FFFF）。
   >  其他极少使用的Unicode 辅助平面的字符使用四至六字节编码（Unicode范围由U+10000至U+1FFFFF使用四字节，Unicode范围由U+200000至U+3FFFFFF使用五字节，Unicode范围由U+4000000至U+7FFFFFFF使用六字节）。

- 故一个中文,将会被转换成长度为6的字符串来表示.

### 三,参考网址

* 1.[utf-8 wikipedia](https://zh.wikipedia.org/wiki/UTF-8)

