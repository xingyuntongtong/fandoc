JS语言自身只有字符串数据类型，没有二进制数据类型，因此NodeJS提供了一个与String对等的全局构造函数Buffer来提供对二进制数据的操作。
## 1. 什么是Buffer
- 它是一个缓冲区Buffer是暂时存放输入输出数据的一段内存。
- JS语言没有二进制数据类型，而在处理TCP和文件流的时候，必须要处理二进制数据。
- NodeJS提供了一个Buffer对象(类)来提供对二进制数据的操作,该类用来创建一个专门存放二进制数据的缓存区
- 是一个表示固定内存分配的全局对象，也就是说要放到缓存区中的字节数需要提前确定
- Buffer好比由一个多位字节元素组成的数组，可以有效的在javascript中表示二进制数据
- 在 Node.js 中，Buffer 类是随 Node 内核一起发布的核心库。Buffer 库为 Node.js 带来了一种存储原始数据的方法，可以让 Node.js 处理二进制数据，每当需要在 Node.js 中处理I/O操作中移动的数据时，就有可能使用 Buffer 库。原始数据存储在 Buffer 类的实例中。一个 Buffer 类似于一个整数数组，但它对应于 V8 堆内存之外的一块原始内存。
- 我们的buffer的表现形式是16进制的
## 2. 什么是字节
- 字节(Byte)是计算机存储时的一种计量单位，一个字节等于8位二进制数
- 一个位就代表一个0或1，每8个位（bit）组成一个字节（Byte）
- 字节是通过网络传输信息的单位
- 一个字节最大值十进制数是255 一个汉字是由3个字节组成的
```
var sum =0;
for(var i=0;i<8;i++){
  sum += Math.pow(2,i);
}
```
## 3. 定义buffer的三种方式
- 通过长度定义buffer,通过长度创建buffer是随机的
```
var buffer = new Buffer(3);
```
- 通过数组定义buffer
```
var buffer = new Buffer([2,3]);
```
正常情况下一个字节表示0-255之间;
如果不识别则为0,如果给的值大于255,会对256取模(%),如果为负值 加上256

- 字符串创建,node中不支持gb(国标)
```
var buffer = new Buffer('名字','utf8');
```
## 4.buffer常用方法
### 4.1 fill方法
- 手动初始化,擦干净桌子,将buffer内容清0
```
buffer.fill(0);
```
### 4.2 write方法
- 往buffer中写入内容,4个参数
```
buffer.write('中',0,3,'utf8');
buffer.write('文',3,3,'utf8'); //中文
```
string 要写入的字符串，一个汉字3个字节
offset 写入的偏移量 就是从哪个字节开始写入 默认为 0 
length 写入的长度 字节的长度
encoding 写入内容的编码格式 默认为 'utf8' 
后两个参数可以不写write方法会默认把内容从头写到尾
- 返回值：返回实际写入的大小。如果 buffer 空间不足， 则只会写入部分字符串。

### 4.3 toString方法
- 将buffer转换成字符串类型 转化时可以根据截取的位置进行输出
```
console.log(buffer.toString('utf8',3,6));
```
第一天参数可以是utf8也可以是base64;后两个参数是截取的buffer的长度
- 返回值：解码缓冲区数据并使用指定的编码返回字符串。

### 4.4 slice方法
- 截取buffer
```
var buffer = new Buffer([1,2,3]);
var newBuffer = buffer.slice(2); //从索引2开始
newBuffer[0] = 100;
console.log(newBuffer); //64
```
区别在于slice后操作的还是原buffer 100转换成16进制为64
- 返回值：返回一个新的缓冲区，它和旧缓冲区指向同一块内存，但是从索引 start 到 end 的位置剪切。

- 截取乱码问题, 编译 string_decoder
```
var StringDecoder  = require('string_decoder').StringDecoder;
var sd = new StringDecoder;
var buffer = new Buffer('中文');
console.log(sd.write(buffer.slice(0,4).toString()));
console.log(sd.write(buffer.slice(4).toString()));
```
先new一个实例 把字符串存进去，不能识别的保留，当在输出的时候进行拼接
如果不编译string_decoder，就会出现:本来中文是6个字节，第一次(0,4)就是说第一个字是完整的，第二个字显示不全就会出现乱码
### 4.5 copy方法
- 复制Buffer 把多个小buffer拷贝到一个大buffer上
```
sourceBuffer.copy(targetBuffer,targetstart,sourcestart,sourceend);
var buffer1 = new Buffer('ABC'); // 拷贝一个缓冲区
var buffer2 = new Buffer(3);
buffer1.copy(buffer2); //把buffer1拷贝到buffer2里
console.log(buffer2.toString());
```
targetBuffer 目标buffer 要拷贝的 Buffer 对象
targetStart 目标的开始 数字, 可选, 默认: 0
sourceStart 拷贝内容的开始 数字, 可选, 默认: 0
sourceEnd 拷贝内容的结束 数字, 可选, 默认: buffer.length
- 返回值：没有返回值

### 4.6 concat方法
- 拼接buffer 将两个buffer拼接成1个buffer 也叫缓冲区合并
```
var buffer1 = new Buffer('中');
var buffer2 = new Buffer('文');
var newBuffer = Buffer.concat([buffer1,buffer2],3);
console.log(newBuffer.toString()); //只会显示中因为长度是3
```
- 实现concat方法  我们要实现一个buffer方法
```
Buffer.myconcat = function (list,totalLength) {
    if(list.length==1){//如果数组里只有一项直接返回即可
        return list[0];
    }
    if(typeof totalLength=='undefined'){//手动维护长度，当没有传长度时
        totalLength = 0;
        for(var j= 0;j<list.length;j++){
            totalLength += list[j].length; //每一个buffer的长度累加算出总长度
        }
    }
    //我们已经拥有长度，需要将每一项粘贴进去
    var buffer = new Buffer(totalLength);
    //将每一个buffer拷贝到buffer中
    var index = 0;//定一个计数器
    list.forEach(function (item) {
        item.copy(buffer,index)//每次拷贝时索引是递增的
        index+=item.length;
    });
    return buffer.slice(0,index); //截取掉想要的内容
    //返回大buffer
};
console.log(Buffer.myconcat([buffer1,buffer2],100).toString());
```
list(参数1) - 用于合并的 Buffer 对象数组列表。
totalLength(参数2) - 指定合并后Buffer对象的总长度。
- 返回值：返回一个多个成员合并的新 Buffer 对象。

### 4.7 isBuffer
- 判断是否是buffer
```
console.log(Buffer.isBuffer(new Buffer(3)));
```
### 4.8 length
- 获取字节长度(显示是字符串所代表buffer的长度)
```
var buffer = Buffer.byteLength("中文");
buffer.length;
```
- 返回值: 返回 Buffer 对象所占据的内存长度。

## 5.进制转换
- 将任意进制字符串转换为十进制
parseInt(“11”, 2); // 3 2进制转10进制
parseInt(“77”, 8); // 63 8进制转10进制
parseInt(“e7”, 16); //175 16进制转10进制
- 将10进制转换为其它进制字符串
(3).toString(2)) // “11” 十进制转2进制
(17).toString(16) // “11” 十进制转16进制
(33).toString(32) // “11” 十提制转32进制
- 2进制是缝二进一，8进制以此类推。。。
- parseInt 将任意进制转换成10进制
- toString 将任意进制转换成任意进制
- 计算过程：从最后一位开始算，比如 77(8进制转10进制)--> 7^8^1+7^8^0 它是7乘以8的0次方+7乘以8的1次方
以此类推都是这样计算，
- 16进制ff在10进制中最大是多少 255 ，0x代表16进制
```
console.log((0xff).toString(2));
```
- 一个字节由8个位组成，每个位不是0 就是1  最高位 1^2^7 2进制

## 6.base64 转换成可见编码(base64实现原理)
Base64是一种用64个字符来表示任意二进制数据的方法。 用记事本打开exe、jpg、pdf这些文件时，我们都会看到一大堆乱码，因为二进制文件包含很多无法显示和打印的字符，所以，如果要让记事本这样的文本处理软件能处理二进制数据，就需要一个二进制到字符串的转换方法。Base64是一种最常见的二进制编码方法。
- 相当于2进制最大为6个1 前面两位为0转换成10进制后为最大63 0-63，这个10进制的值就代表数组的索引
- base64,在0-63之间随机取可见编码
- ABCDEFGHIJKLMNOPQRSTUVWXYZ
- abcdefghijklmnopqrstuvwxyz
- 0123456789
- +/
```
var buffer = new Buffer('珠'); //默认buffer为16进制
console.log(buffer); //e7 8f a0 此时是16进制
//将珠字转换成base64
//将16进制转换成2进制  0x代表16进制
console.log((0xe7).toString(2));
console.log((0x8f).toString(2));
console.log((0xa0).toString(2));
//11100111   10001111   10100000 都大于64 因为前两位里面有1
//转换成小于64的
//规则:将所有的2进制进行拼接，每隔6位分割 前面加00 就是先都放到一起在分割，因为每个字节8个位，一个汉字3个字节，所以是24个位，每6个一组是它不会超过64肉眼可以看到
//00111001 00111000 00111110 00100000
//3个8位转成4个六位
//转换成10进制进行取值
console.log(parseInt('00111001',2));
console.log(parseInt('00111000',2));
console.log(parseInt('00111110',2));
console.log(parseInt('00100000',2));
//57 56 62 32 得到的数字就是索引
var str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
str+='abcdefghijklmnopqrstuvwxyz';
str+='0123456789';
str+='+/';
var base64 = str[57]+str[56]+str[62]+str[32];
console.log(base64);//54+g
```
- 得出结果是54+g;在工具里过一下就会得出珠字 base64不是加密的;
- 把base64封装成方法
```
function toBase64 (str) {
    //先把字符串转成数组 最终返回的结果是这些字符组成的
    var chars='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'.split('');
    var buffer=new Buffer(str);//通过字符串构建buffer
    var str='';
    for (var i=0;i<buffer.length;i++) {
        //把Buffer中的值转成二进制字符串，拼在一起
        var a= buffer[i].toString(2);
        str+=a;
    }
    //中国 6个字节 48位  48/6=8
    var result='';
    str=str.replace(/\d{6}/g,function  (matched) {
        //matched是六位一组的字符子串
        //parseInt负责把它们转成10进制数
        //chars从字符数组中按索引取出对应的字符，拼在一起
        result+=chars[parseInt(matched,2)];
    });
    return result;
}
console.log(toBase64('中国'));
```
### 7. DataURL
Data URL给了我们一种很巧妙的将图片“嵌入”到HTML中的方法。跟传统的用img标记将服务器上的图片引用到页面中的方式不一样，在Data URL协议中，图片被转换成base64编码的字符串形式，并存储在URL中，冠以mime-type
- 当访问外部资源很麻烦或受限时
- 当图片是在服务器端用程序动态生成，每个访问用户显示的都不同时。比如验证码
- 当图片的体积太小，占用一个HTTP会话不是很值得时。比如小图标，验证码，把它内嵌到页面中
```
<img src="data:image/gif;base64,R0lGO">
```
R0lGO 就是base64码


