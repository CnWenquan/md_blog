---
title: php+jquery处理excel文件异步读取
url: 319.html
id: 319
categories:
  - CI
  - PHP
date: 2016-10-28 15:04:03
tags:
---

首先，准备工作：

PHPExcel.php文件、JQuery.js文件、ajaxFileUpload.js  

Html界面：

    <input class="upbut" type="file" enctype="multipart/form-data" name="file" id="uploadFile">

引入jquery与ajaxFileUpload文件

  

* * *

Js：

    $('#uploadFile').change(function(){  //此处用了change事件，当选择好图片打开，关闭窗口时触发此事件
        var filename = $("#uploadFile").val();
        if(filename.length>0){
            $.ajaxFileUpload({
                url:'/UploadTest/',   //处理图片的脚本路径
                type: 'post',       //提交的方式
                secureuri:false,
                fileElementId:'uploadFile',
                dataType: 'json',
                success : function (data, status){  //提交成功后自动执行的处理函数
                    console.log(data);
                    if(data.error==null){
                        $('#add_cinema').hide();
                        $('#edit_cinema').show();
                        var reg = new RegExp(",","g");
                        $('#txtArea').val(data.data.replace(reg,"\\n"));
                        
                    }else{
                        alert(data.error);
                    }
                },
                error: function(data, status, e){   //提交失败自动执行的处理函数
                    alert(e);
                }
            });
        }
    });

Controller：

    public function index(){

        $whitelist = array('xlsx');
        $name      = NULL;
        $error     = '文件上传失败！';
        $data      = '';
        if(isset($_FILES)){
            if(isset($_FILES\['file'\])){
                    $tmp\_name = $\_FILES\['file'\]\['tmp_name'\];
                    $name     = basename($_FILES\['file'\]\['name'\]);
                    $error    = $_FILES\['file'\]\['error'\];

                    if ($error === UPLOAD\_ERR\_OK) {
                            $extension = pathinfo($name, PATHINFO_EXTENSION);
                            if (!in_array($extension, $whitelist)) {
                                    $error = '请上传excel文件！';
                            } else {
                                if(!file\_exists($tmp\_name)){
                                    $error = '未找到文件！';
                                }else{
                                    $objReader = PHPExcel\_IOFactory::createReaderForFile($tmp\_name);
                                    $PHPExcel = $objReader->load($tmp_name);
                                    $sheet = $PHPExcel->getSheet(0); // 读取第一個工作表
                                    $highestRow = $sheet->getHighestRow(); // 取得总行数
                                    $highestColumm = $sheet->getHighestColumn(); // 取得总列数
                                    /** 循环读取每个单元格的数据 */
                                    for ($row = 1; $row <= $highestRow; $row++){//行数是以第1行开始
                                        for ($column = 'A'; $column <= $highestColumm; $column++) {//列数是以A列开始
                                            $dataset\[\] = $sheet->getCell($column.$row)->getValue();
                                            $data= $data.$sheet->getCell($column.$row)->getValue()." ";
                                        }
                                        $data=$data.",";
                                    }
                                    $error = NULL;
                                }  
                            }
                    }
            }
        }
        echo json_encode(array(
                'name'  => $name,
                'error' => $error,
                'data'  => $data,
        ));
    }