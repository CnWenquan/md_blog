---
title: php 7.1 mcrypt aes 加密替换为openssl 加密
tags:
  - PHP
url: 737.html
id: 737
categories:
  - PHP
date: 2018-01-29 12:48:56
---

mcrypt

* * *

/**  
 \* 对问答信息进行签名 \* @param $info  
 */function signQuestion($info) {  
 $key = signKey();  
//    $key_size =  strlen($key);  
//    echo "Key size: " . $key_size . "\\n";  
  
 $plaintext = json_encode($info);  
  
 \# 为 CBC 模式创建随机的初始向量  
 $iv_size = mcrypt\_get\_iv_size(MCRYPT\_RIJNDAEL\_128, MCRYPT\_MODE\_CBC);  
 $iv = mcrypt\_create\_iv($iv_size, MCRYPT_RAND);  
  
  
 \# 创建和 AES 兼容的密文（Rijndael 分组大小 = 128）  
 # 仅适用于编码后的输入不是以 00h 结尾的 # （因为默认是使用 0 来补齐数据） $ciphertext = mcrypt_encrypt(MCRYPT\_RIJNDAEL\_128, $key,  
 $plaintext, MCRYPT\_MODE\_CBC, $iv);  
  
 \# 将初始向量附加在密文之后，以供解密时使用  
 $ciphertext = $iv . $ciphertext;  
 \# 对密文进行 base64 编码  
 $ciphertext_base64 = base64_encode($ciphertext);  
 return $ciphertext_base64;  
}  
  
/**  
 \* 将字符串解开，得到问答信息  
 \* @param $ciphertext  
 */  
function unsignQuestion($ciphertext_base64) {  
    $key = signKey();  
  
  
    # === 警告 ===  
    # 密文并未进行完整性和可信度保护，  
    # 所以可能遭受 Padding Oracle 攻击。  
    # \-\-\- 解密 ---  
  
    $ciphertext\_dec = base64\_decode($ciphertext_base64);  
  
    $iv\_size = mcrypt\_get\_iv\_size(MCRYPT\_RIJNDAEL\_128, MCRYPT\_MODE\_CBC);  
    # 初始向量大小，可以通过 mcrypt\_get\_iv_size() 来获得  
    $iv\_dec = substr($ciphertext\_dec, 0, $iv_size);  
  
    # 获取除初始向量外的密文  
    $ciphertext\_dec = substr($ciphertext\_dec, $iv_size);  
  
    # 可能需要从明文末尾移除 0  
    $plaintext\_dec = mcrypt\_decrypt(MCRYPT\_RIJNDAEL\_128, $key,  
        $ciphertext\_dec, MCRYPT\_MODE\_CBC, $iv\_dec);  
  
    return $plaintext_dec;  
}  

  

  

  

openssl

* * *

/**  
 \* 对问答信息进行签名 \* @param $info  
 */function signQuestion($info) {  
 $key = signKey();  
//    $key_size =  strlen($key);  
//    echo "Key size: " . $key_size . "\\n";  
  
 $plaintext = json_encode($info);  
 $cipher = "aes-128-gcm";  
 if (in_array($cipher, openssl\_get\_cipher_methods()))  
 { $ivlen = openssl\_cipher\_iv_length($cipher);  
 $iv = openssl\_random\_pseudo_bytes($ivlen);  
 $ciphertext = openssl_encrypt($plaintext, $cipher, $key, $options=0, $iv, $tag);  
 //store $cipher, $iv, and $tag for decryption later  
 $original_plaintext = openssl_decrypt($ciphertext, $cipher, $key, $options=0, $iv, $tag);  
 return $original_plaintext;  
 }else{  
 return false;  
 }}  
  
/**  
 \* 将字符串解开，得到问答信息  
 \* @param $ciphertext  
 */  
function unsignQuestion($ciphertext_base64) {  
    $key = signKey();  
  
    $cipher = "aes-128-gcm";  
    if (in\_array($cipher, openssl\_get\_cipher\_methods())) {  
        $ivlen = openssl\_cipher\_iv_length($cipher);  
        $iv = openssl\_random\_pseudo_bytes($ivlen);  
        $original\_plaintext = openssl\_decrypt($ciphertext_base64, $cipher, $key, $options = 0, $iv);  
        return $original_plaintext;  
    }else{  
        return false;  
    }  
}