---
title: Implementing the openssl_encrypt method of PHP using Java
permalink: Implementing-the-openssl_encrypt-method-of-PHP-using-Java
categories:
- program
tags: 
- notes
comments: true
date: 2019-09-11
updated: 2019-09-11
---

使用 Java 实现 PHP openssl_encrypt加密方法


## PHP Implement

```php
    public static function encrypt($input, $key)
    {
        $data = openssl_encrypt($input, 'AES-128-ECB', $key, OPENSSL_RAW_DATA);
        $data = base64_encode($data);
        return $data;
    }
    
	public static function decrypt($input, $key)
	{
		$decrypted = openssl_decrypt(base64_decode($input), 'AES-128-ECB', $key, OPENSSL_RAW_DATA);
		return $decrypted;
	}
```

## Java Implement

```java
    public static String encrypt(String data, String password) throws Exception {
        byte[] key = new byte[16];
        for (int i = 0; i < 16; i++) {
            if (i < password.getBytes().length) {
                key[i] = password.getBytes()[i];
            } else {
                key[i] = 0;
            }
        }

        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(key, "AES"));

        return Base64.getEncoder().encodeToString(cipher.doFinal(data.getBytes()));
    }
```

注意： 如果采用的是 `AES-128-CBC`，那么 Java Implement 则要改成

```java
    public static String encrypt(String data, String password, String iv) throws Exception {
        byte[] key = new byte[16];
        for (int i = 0; i < 16; i++) {
            if (i < password.getBytes().length) {
                key[i] = password.getBytes()[i];
            } else {
                key[i] = 0;
            }
        }

        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
//        byte[] ivs= {0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0};
        cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(key, "AES"),
                new IvParameterSpec(iv.getBytes()));

        return  Base64.getEncoder().encodeToString(cipher.doFinal(data.getBytes()));
    }
```