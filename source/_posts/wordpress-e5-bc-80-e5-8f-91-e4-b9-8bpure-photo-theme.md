---
title: wordpress 开发之pure photo theme
url: 285.html
id: 285
categories:
  - PHP
date: 2016-10-11 16:31:11
tags:
---

    pure photo theme是一个照片相册、图片展示的主题，鄙人是通过网上搜索认识的，一个国外开发的模板，英语底子薄所以遇到了不少坑。

需要解决的问题：

    1、网站需要上传大量的图片

     找插件，诶呦不错哦，找到了一个：WP Smush，免费的，无损压缩，网上各种推荐，那就装个呗，安装后才发现，每张图片都超过了最大限制，没有一张被瘦身，o my god!! 于是就想办法先让图片小瘦身，然后再用插件瘦身，于是就用到了linux定时任务，每小时跑一次（办法比较笨，不熟悉WordPress上传机制，只好到服务器目录里操作，待改进）

    2、媒体库没有分类目录（我也是醉了）

    起初以为这种小问题，不能有插件吧，于是网上找demo，找到了，坑爹的是在left侧边栏加上了分类条目了，但媒体库里的导航条里木有，这不是白做吗！！！于是找插件，定位是直接可以生成相册的那种，于是找到个NextGEN Gallery、Easy Image Gallery等插件，一一试过了，他们的工作原理就是脱离了系统自带的媒体库，自己独立创建一套，你直接在它提供的上传图片地址操作，上传服务器目录也不是系统自带的目录，但有一点好处就是你要创建文件、画廊gallery时可以引入编辑框，发布文章可以定义展示效果等。可以说很方便。但我想说的是，这么多功能管我屁事儿！！！我的奇葩的模板必须通过画廊的items创建展示内容，而且不能选系统多媒体图片以外的！！  

    早知有这么复杂，还不如用这些第三方插件算了！但我觉得程序员要有一种极客精神，既然我都认可这套模板了，干嘛还想其他的。

    问题回到给媒体设置分类的起点，何不查下呢，一查还真有，用的插件是Media Library Categories,很不错，给媒体单独加了个分类目录，在媒体库也可以批量操作。但缺点是在gallery添加items的时候，木有分类了。

    3、幻灯片的图片无法展示

    看着网上的demo，幻灯片酷炫的效果也是这套模板的亮点，所以看着我的黑乎乎的界面，还挺窝心的，于是撸起袖子，说干就干，我在wordpress4.5.3中文版做过一套，是幻灯片可以展示其他有问题，而目前这套是4.6.1英文版，先把项目打包了一份，防止改动太大还得从来，那就悲催了，等一切准备工作完成后，先比较了下两版同样的幻灯片目录是否相同，一毛一样！！心里安慰是其他文件有变动的原因，于是上传服务器等待奇迹出现的一刻，好不容易更完了，强刷界面，升级数据库，还TM是老样子，那是不是老数据信息不全呢，于是新增了一个画廊，满怀期待的看结果，还是老样子！！！然后是比较其他有图片的界面，左右对比了半天，全局搜索了半天关键词，发现有两种获取图片的方法，一个是通过page\_id获取的，一个是通过gallery\_id获取的，当前页是gallery\_id获取的 ，返回时null，于是换page\_id，返回的是false。MD

    看数据库结构，为毛不给我返回了，主要看post跟postmeta两张表。post里有图片地址，但人家是属于媒体post的时候保留的，跟gallery没关系！！！根据gallery\_id查找postmeta看出点儿门道，有每个item的post\_id，于是就想到用他的第一个item的post\_id查图片，然后展示。思路算是有了，但试了几次好像不能在page\_XXX.php里用全局变量$wpdb，于是就近看了下其他的全局变量是在哪儿定义的，也跟着写在同一个文件里。然后调用后木有问题了。

附上获取gallery_img图片的方法：

function get\_gallery\_img($id){
    global $wpdb;
    $img\_id = $wpdb->get\_row("select meta\_value from $wpdb->postmeta where post\_id = ".$id." and meta\_key ='gallery\_items\_0\_image'")->meta_value;
    $img\_info = $wpdb->get\_row("select meta\_value from $wpdb->postmeta where post\_id = ".$img\_id." and meta\_key = '\_wp\_attached_file'");
    return "http://media.wenquanpage.com/wp-content/uploads/".$img\_info->meta\_value;
}