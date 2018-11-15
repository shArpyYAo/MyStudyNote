我们先来看一份代码

    function download(content, filename) {
        // 字符内容转变成blob地址
        var blob = new Blob([content]);
        var eleLink = document.createElement('a');
        eleLink.download = filename;
        $(eleLink).css('display', 'none');
        eleLink.href = URL.createObjectURL(blob);
        document.body.appendChild(eleLink);
        eleLink.click();
        document.body.removeChild(eleLink);
    };

上面的代码的意思是先用blob把字符串内容转变成blob链接，然后利用a标签自带的下载功能把内容下载下来。

以上代码在Chrome、Firefox、Safari、360、EdgeHtml浏览器中，均可以成功下载文件，但是在Edge中，会报错。如下：

<center style="color:red">SCRIPT: 拒绝访问</center>

造成以上原因是在Edge中使用Blob生成的是不带域名的blob链接，如下：

<center style="color:red">blob:00F0B45-DD4E-4F4F-9B15-000368F15E20</center>

而chrome等浏览器生成的是带域名的，如下：

<center style="color:red">http://xxx.xxx.biz/86e01467-6654-4b74-98b3-ca25f396bc2f</center>

所以在edge下通过a标签的href来下载是不行的。

# 解决方法

使用 window.navigator.msSaveOrOpenBlob(blob, filename)，代替 window.URL.createObjectURL。

上代码：

    function download(content, filename) {
        // 字符内容转变成blob地址
        var blob = new Blob([content]);
        if('msSaveOrOpenBlob' in navigator){
            window.navigator.msSaveOrOpenBlob(blob, filename);
            return;
        }
        var eleLink = document.createElement('a');
        eleLink.download = filename;
        $(eleLink).css('display', 'none');
        eleLink.href = URL.createObjectURL(blob);
        document.body.appendChild(eleLink);
        eleLink.click();
        document.body.removeChild(eleLink);
    };
    
