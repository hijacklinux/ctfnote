---
layout: default
---
# ![](../img/hj.jpg)DES解密源码（C#）

```C#
using System;
using System.Text;
using System.Security.Cryptography;
using System.IO;
namespace RectangleApplication
{
    class Rectangle
    {
           public string Decode()
    {
                string result;
                byte[] byte1;

                    byte1 = Convert.FromBase64String("fOCPTVF0diO+B0IMXntkPoRJDUj5CCsT");
                    byte[] bytes = Encoding.ASCII.GetBytes("wctf{wol");
                    byte[] bytes2 = Encoding.ASCII.GetBytes("dy_crack}");
                    DESCryptoServiceProvider dESCryptoServiceProvider = new DESCryptoServiceProvider();
                    MemoryStream memoryStream = new MemoryStream();
                    CryptoStream cryptoStream = new CryptoStream(memoryStream, dESCryptoServiceProvider.CreateDecryptor(bytes, bytes2), CryptoStreamMode.Write);
                    cryptoStream.Write(byte1, 0, byte1.Length);
                    cryptoStream.FlushFinalBlock();
                    System.Text.Encoding encoding = System.Text.Encoding.UTF8;
                    result = encoding.GetString(memoryStream.ToArray());


				Console.Write(result);
				return result;
    }
    }

    class ExecuteRectangle
    {
        static void Main(string[] args)
        {
            Rectangle r = new Rectangle();


            r.Decode();
            Console.ReadLine();
        }
    }
}
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
