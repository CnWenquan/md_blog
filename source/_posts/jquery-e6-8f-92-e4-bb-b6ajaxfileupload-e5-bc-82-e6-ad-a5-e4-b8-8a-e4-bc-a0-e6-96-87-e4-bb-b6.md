---
title: JQuery插件ajaxFileUpload 异步上传文件
url: 315.html
id: 315
categories:
  - PHP
date: 2016-10-27 17:03:59
tags:
---

一、先对ajaxFileUpload插件的语法参数进行讲解

　　原理：ajaxfileupload是通过监听iframe的onload方法来实现， 当从服务端处理完成后，就触发iframe的onload事件调用其绑定的方法，在绑定的方法中获取iframe中服务器返回的数据体（支持的普通文本,json,xml,script, html）

　　语法：$.ajaxFileUpload(\[options\])

　　参数说明：

　　1，url　　　　　　　　　　  上传处理程序地址，也就是我发送给服务器端所要处理上传的地址。　　  
　　2，fileElementId　　　　　  需要上传的文件域的ID，即<input type="file" id="file">的ID。  
　　3，secureuri　　　　　　　 是否启用安全提交，默认为false。  
　　4，dataType　　　　　　　  服务器返回的数据类型。可以为xml,script,json,html。如果不填写，jQuery会自动判断。  
　　5，success　　　　　　　　提交成功后自动执行的处理函数，参数data就是服务器返回的数据。  
　　6，error　　　　　　　　　 提交失败自动执行的处理函数。  
　　7，data 　　　　　　　　　 自定义参数。这个很有用，比如你上传图片的同时想把图片名也一起传过去，可以用这个参数去实现。  
　　8，type 　　　　　　　　　 当要提交自定义参数时，这个参数要设置成post

　　二、接下来我们看看如何去使用

　　1、先引入ajaxFileUpload这个插件。

<script src="jquery-1.11.1.js" type="text/javascript"></script><script src="ajaxfileupload.js" type="text/javascript"></script>

　　这里我用的是jq1.11.1版本，网上有说jq版本与ajaxfileupload的版本要对应才不会有异常报错，反正我现在这个没错误。

　　2、贴上HTML的代码。

        <div data-role="fieldcontain" class="upload-box">
                <label for="id_photos"><span class="red">* </span>您的有效证件照：</label>
                    <input type="file" id="id\_photos" name="id\_photos" value="上传" style="filter:alpha(opacity=10);-moz-opacity:10;opacity:10;" />            
                <p style="margin-top:0.5em;color:#999;font-size:11pt;">说明：请上传手持证件的半身照，请确保照片内证件信息清晰可读。</p>
        </div>
        <div class="id_photos" >
            
        </div>

  

　　此处主要的是<input type="file" id="id\_photos" name="id\_photos" value="上传">这一句代码，其他的不用管，因为这里我是在手机端，用的是jqueryMobile插件。

　　3、到js代码进行处理。

$(document).bind('pageinit', function(){    //照片异步上传
    $('#id_photos').change(function(){  //此处用了change事件，当选择好图片打开，关闭窗口时触发此事件        $.ajaxFileUpload({
        url:'/uploader/',   //处理图片的脚本路径
        type: 'post',       //提交的方式
        secureuri :false,   //是否启用安全提交
        fileElementId :'id_photos',     //file控件ID
        dataType : 'json',  //服务器返回的数据类型      
        success : function (data, status){  //提交成功后自动执行的处理函数
            if(1 != data.total) return;　　 //因为此处指允许上传单张图片，所以数量如果不是1，那就是有错误了
            var url = data.files\[0\].path;　　
            $('.id\_photos').empty();            //此处效果是：当成功上传后会返回一个json数据，里面有url，取出url赋给img标签，然后追加到.id\_photos类里显示出图片
            $('.id_photos').append('<img src="'+url+'" value="'+url+'" style="width:80%" >');            //$('.upload-box').remove();        },
        error: function(data, status, e){   //提交失败自动执行的处理函数            alert(e);
        }
    })
});

  

　　这里我对每一行的代码都基本写上了注释方便大家理解。流程就是上传图片给uploader.php去处理，处理成功返回json数据，然后在json中取出url值，将其赋给img标签里，然后将img标签追加带页面显示出来。

　　这里我附上json返回的数据：

  

{    "total": 1,    "success": 1,    "files": \[
        {            "srcName": "3.jpg",            "error": 0,            "size": 10715,            "type": "image/jpeg",            "success": true,            "path": "http://m.kellyblog.com/upload/20150528/857f4a35664b4a665e713322306d73b2.0x124x126.jpg",            "width": 124,            "height": 126
        }
    \]
}

  

　　上传前HTML页面是这样的：

![](/ueditor/php/upload/image/20161027/1477558877880239.jpg)

　　异步上传成功后HTML页面效果是这样子的：

　　![](/ueditor/php/upload/image/20161027/1477558877981758.jpg)

　　4、看看PHP是如何处理的

  

class UploaderController extends XXXX_Controller {
    public function index() {
        $files = array();
        $success = 0;    //用户统计有多少张图片上传成功了
        
        foreach ($_FILES as $item) {
            $index = count($files);

            $files\[$index\]\['srcName'\] = $item\['name'\];    //上传图片的原名字
            $files\[$index\]\['error'\] = $item\['error'\];    //和该文件上传相关的错误代码
            $files\[$index\]\['size'\] = $item\['size'\];        //已上传文件的大小，单位为字节
            $files\[$index\]\['type'\] = $item\['type'\];        //文件的 MIME 类型，需要浏览器提供该信息的支持，例如"image/gif"
            $files\[$index\]\['success'\] = false;            //这个用于标志该图片是否上传成功
            $files\[$index\]\['path'\] = '';                //存图片路径

            // 接收过程有没有错误
            if($item\['error'\] != 0) continue;
            //判断图片能不能上传
            if(!is\_uploaded\_file($item\['tmp_name'\])) {
                $files\[$index\]\['error'\] = 8000;
                continue;
            }
            //扩展名
            $extension = '';
            if(strcmp($item\['type'\], 'image/jpeg') == 0) {
                $extension = '.jpg';
            }
            else if(strcmp($item\['type'\], 'image/png') == 0) {
                $extension = '.png';
            }
            else if(strcmp($item\['type'\], 'image/gif') == 0) {
                $extension = '.gif';
            }
            else {
                //如果type不是以上三者，我们就从图片原名称里面去截取判断去取得(处于严谨性)    
                $substr = strrchr($item\['name'\], '.');
                if(FALSE == $substr) {
                    $files\[$index\]\['error'\] = 8002;
                    continue;
                }

                //取得元名字的扩展名后，再通过扩展名去给type赋上对应的值
                if(strcasecmp($substr, '.jpg') == 0 || strcasecmp($substr, '.jpeg') == 0 || strcasecmp($substr, '.jfif') == 0 || strcasecmp($substr, '.jpe') == 0 ) {
                    $files\[$index\]\['type'\] = 'image/jpeg';
                }
                else if(strcasecmp($substr, '.png') == 0) {
                    $files\[$index\]\['type'\] = 'image/png';
                }
                else if(strcasecmp($substr, '.gif') == 0) {
                    $files\[$index\]\['type'\] = 'image/gif';
                }
                else {
                    $files\[$index\]\['error'\] = 8003;
                    continue;
                }
                $extension = $substr;
            }

            //对临时文件名加密，用于后面生成复杂的新文件名
            $md5 = md5\_file($item\['tmp\_name'\]);
            //取得图片的大小
            $imageInfo = getimagesize($item\['tmp_name'\]);
            $rawImageWidth = $imageInfo\[0\];
            $rawImageHeight = $imageInfo\[1\];

            //设置图片上传路径，放在upload文件夹，以年月日生成文件夹分类存储，
            //rtrim(base_url(), '/')其实就是网站的根目录，大家自己处理
            $path = rtrim(base_url(), '/') . '/upload/' . date('Ymd') . '/';
            //确保目录可写
            ensure\_writable\_dir($path);
            //文件名
            $name = "$md5.0x{$rawImageWidth}x{$rawImageHeight}{$extension}";
            //加入图片文件没变化到，也就是存在，就不必重复上传了，不存在则上传
            $ret = file\_exists($path . $name) ? true : move\_uploaded\_file($item\['tmp\_name'\], $serverPath . $name);
            if($ret === false) {
                $files\[$index\]\['error'\] = 8004;
                continue;
            }
            else {
                $files\[$index\]\['path'\] = $path . $name;        //存图片路径
                $files\[$index\]\['success'\] = true;            //图片上传成功标志
                $files\[$index\]\['width'\] = $rawImageWidth;    //图片宽度
                $files\[$index\]\['height'\] = $rawImageHeight;    //图片高度
                $success ++;    //成功+1
            }
        }

        //将图片已json形式返回给js处理页面  ，这里大家可以改成自己的json返回处理代码
        echo json_encode(array(
            'total' => count($files),
            'success' => $success,
            'files' => $files,
        ));
    }
}
/*********************************分割*************************************************/
//这里我附上ensure\_writable\_dir()函数的代码
/**
* 确保文件夹存在并可写
*
* @param string $dir
*/
function ensure\_writable\_dir($dir) {
    if(!file_exists($dir)) {
        mkdir($dir, 0766, true);
        chmod($dir, 0766);
        chmod($dir, 0777);
    }
    else if(!is_writable($dir)) {
        chmod($dir, 0766);
        chmod($dir, 0777);
        if(!is_writable($dir)) {
            throw new FileSystemException("目录 $dir 不可写");
        }
    }
}

　　 代码基本上都加上了注释，方便大家理解，虽然是用PHP处理图片上传，但你理解了上传时程序代码所处理的逻辑思路，将思路用于.net或者java里都还是可以的。

 　　以上就是使用JQuery插件ajaxFileUpload 异步上传文件的整一个分析过程。

　　网上找这个ajaxfileupload插件比较杂，所以这里给个链接大家去下载：

　　　　[http://pan.baidu.com/s/1gdF5CdH](http://pan.baidu.com/s/1gdF5CdH)