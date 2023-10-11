# EPZ

添加了自己的报头逻辑
不能直接解压 Lz4 ，LZMA，LZString 文件

```javascript
// 文件详细
let arr = [69,80,90,62,
           0,0,0,45,
           2,
           3,
           104,101,108,108,111,47,51,46,99,0,
           0,
           11,
           104,101,108,108,111,47,104,101,108,108,111,46,116,120,116,0,
           0,
           1,
           1,
           104,101,108,108,111,47,49,47,50,46,99,0,
           49,49,49,
           72,101,108,108,111,32,87,111,114,108,100
        ]
// 69 80 90 62  EPZ>
// 魔法数字     
// EPZ>     从头部读取文件信息，文件名信息不压缩
// EPZ\x01  从头部读取文件信息，使用 LZMA 压缩文件名信息，  
// EPZ\x02  从尾部读取文件信息，文件名信息不压缩
// EPZ\x03  从尾部读取文件信息，使用 LZMA 压缩文件名信息
// 00 00 00 45 
//  信息头长度为 45 如果为EPZ\x02或者EPZ\x03 则该信息在最后四位 后面数字均采用VLQ编码 
//  2 
//  两个文件  ----   
// new TextEncoder().encode(new Array(150000).fill(0).map((v,i)=>String.fromCodePoint(i+1)).join("")).indexOf(0)  -1
// 正常 UTF-8 编码的文件名不会出现\0 
//  (Vlq代码来源) http://rosettacode.org/wiki/Variable-length_quantity
//  0:不压缩  1:LZMA 压缩  2:采用 LZ4 压缩  3:采用 LZString 压缩(必须文字格式) 目前只支持这四种 
// ---- 大小为 3           第一个文件 hello/3.c  			      压缩方式为不压缩
//    	3           104,101,108,108,111,47,51,46,99,0     		           0 
// ---- 大小为 11            第二个文件 'hello/hello.txt      		      压缩方式为不压缩
//        11	 104,101,108,108,111,47,104,101,108,108,111,46,116,120,116,0  	0
// 1 
// 引用个数为 1
// 若引用为 0 则表示为空目录，否则则和第i个文件内容是相同的
// ---- 链接文件                               hello/1/2.c                                  其内容和第一个文件 hello/3.c 一致
//       1 (第一个文件)            104,101,108,108,111,47,49,47,50,46,99,0
// 内容
//  49,49,49 文件一的内容  111
//  72,101,108,108,111,32,87,111,114,108,100 文件二的内容 Hello World
// 注释 附加信息任意长度（通常不存在）
```
# 引用
### LZMA
[https://github.com/axutebils74/lz](https://github.com/axutebils74/lz)
### MD5
[https://github.com/emn178/js-md5](https://github.com/emn178/js-md5)
### LZString
[https://github.com/pieroxy/lz-string/](https://github.com/pieroxy/lz-string/)
### LZ4JS
[https://github.com/Benzinga/lz4js/](https://github.com/Benzinga/lz4js/)
### JSZIP
[https://github.com/Stuk/jszip](https://github.com/Stuk/jszip)
