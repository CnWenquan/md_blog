---
title: php 人脸识别Face++
tags:
  - PHP
url: 356.html
id: 356
categories:
  - CI
  - PHP
date: 2016-11-14 17:41:48
---

玩一些听起来高大上的东西，

face++其实对处理的图片进行了分组，分组里有person_name，还有标签等模块

附几张图：

注册成为face++开发者就不说了，进入个人应用平台添加应用也不说了，创建完应用在我的应用中点击管理  

![QQ截图20161114172622.png](/ueditor/php/upload/image/20161114/1479115634490892.png "1479115634490892.png")

  

然后见app管理：

![QQ截图20161114172638.png](/ueditor/php/upload/image/20161114/1479115675773260.png "1479115675773260.png")

如果没有分组，在group下只有一个ungrouped，提前创建了个，所以截图多了个sample_group

![QQ截图20161114172452.png](/ueditor/php/upload/image/20161114/1479115761791338.png "1479115761791338.png")

点开创建的sample_group会看到一个ｇｒｏｕｐ成员

![QQ截图20161114172514.png](/ueditor/php/upload/image/20161114/1479115865604680.png "1479115865604680.png")

点击任意一个成员，会看到每个成员里包含的图片信息：

![QQ截图20161114172537.png](/ueditor/php/upload/image/20161114/1479115907435686.png "1479115907435686.png")![QQ截图20161114172557.png](/ueditor/php/upload/image/20161114/1479115914153549.png "1479115914153549.png")

至此，实现的效果就展示完了：ｃｏｐｙ代码：

* * *

一个ｍｏｄｅｌ，一个ｃｏｎｔｒｏｌｌｅｒ：

ＦａｃｅＰＰＣｌｉｅｎｔ类：

<?php
define ("DEBUG_MODE", true);

class FacePPClient
{	
	private $api\_server\_url;
	private $auth_params;

	public function \_\_construct($api\_key, $api_secret)
	{
		$this->api\_server\_url = "http://api.cn.faceplusplus.com/v2/";
                $this->auth_params = array();
   		$this->auth\_params\['api\_key'\] = $api_key;
   		$this->auth\_params\['api\_secret'\] = $api_secret;
	}
	
	//////////////////////////////////////////////////////////
	// public mathods
	//////////////////////////////////////////////////////////
	
	public function person\_create($person\_name) 
	{
		return $this->call("person/create", array("person\_name" => $person\_name));
	}
	
	public function person\_delete($person\_name)
	{
		return $this->call("person/delete", array("person\_name" => $person\_name));
	}
    
	public function person\_add\_face($face\_id, $person\_name) 
	{
		return $this->call("person/add\_face", array("person\_name" => $person_name,
														 "face\_id" => $face\_id));
	}
	
	public function train\_identify($group\_name) 
	{
		return $this->call("train/identify", array("group\_name" => $group\_name));
	}
	
	public function face_detect($urls = null)
	{
		return $this->call("detection/detect", array("url" => $urls));
	}
	
	public function recognition\_identify($url, $group\_name) 
	{
		return $this->call("recognition/identify", array("url" => $url,
														  "group\_name" => $group\_name));
	}
	
	public function group\_create($group\_name)
	{
		return $this->call("group/create", array("group\_name" => $group\_name));
	}

	public function group\_delete($group\_name)
	{
		return $this->call("group/delete", array("group\_name" => $group\_name));
	}
	
	public function group\_add\_person($person\_name, $group\_name) 
	{
		return $this->call("group/add\_person", array("person\_name" => $person_name,
													  "group\_name" => $group\_name));
	}
    
    public function info\_get\_session($session_id) {
		return $this->call("info/get\_session", array("session\_id" => $session_id));
        
    }
    
    public function face\_detect\_post($filename)
    {
        return $this->post_call("detection/detect", array(
                                  "img" => '@'.$filename
                                  ));
    }
	
	//////////////////////////////////////////////////////////
	// private mathods
	//////////////////////////////////////////////////////////
	
    protected function call($method, $params = array())
    {
    	$params = array\_merge($this->auth\_params, $params);
		$url = $this->api\_server\_url . "$method?".http\_build\_query($params);
		
		if (DEBUG_MODE)
		{
//			echo "REQUEST: $url" . "\\n";
		}
		
    	$ch = curl_init();
    	curl\_setopt($ch, CURLOPT\_URL, $url);
    	curl\_setopt($ch, CURLOPT\_RETURNTRANSFER, true);
     	$data = curl_exec($ch);
    	curl_close($ch);    
    	
		$result = null;
		if (!empty($data))
		{
			if (DEBUG_MODE)
			{
//				echo  $data;
			}
			$result = json_decode($data);
		}
		
		return $result;

    }
    
    protected function post_call($method, $params = array())
    {
    	$params = array\_merge($this->auth\_params, $params);
		$url = $this->api\_server\_url . "$method";
        
		if (DEBUG_MODE)
		{
			echo "REQUEST: $url?" .http\_build\_query($params)."\\n";
		}
        
    	$ch = curl_init();
    	curl\_setopt($ch, CURLOPT\_URL, $url);
    	curl\_setopt($ch, CURLOPT\_POST, 1);
    	curl\_setopt($ch, CURLOPT\_POSTFIELDS, $params);
    	curl\_setopt($ch, CURLOPT\_RETURNTRANSFER, true);
     	$data = curl_exec($ch);
    	curl_close($ch);
        
		$result = null;
		if (!empty($data))
		{
			if (DEBUG_MODE)
			{
				echo "RETURN: " . $data . "\\n";
			}
			$result = json_decode($data);
		}
		
		return $result;
    }
    
}
?>

  

ＦａｃｅＴｅｓｔ控制器：（调用ＦａｃｅＰＰＣｌｉｅｎｔ类）

<?php

defined('BASEPATH') OR exit('No direct script access allowed');
require\_once \_\_DIR__ . '/facepp/FacePPClient.php';

class FaceTest extends CI_Controller {

    function __construct() {
        parent::__construct();
        header('Content-Type:text/html;charset=utf-8');
    }

    public function index() {
        // your api\_key and api\_secret
        $api_key = "＊＊＊＊＊＊＊＊＊";
        $api_secret = "＊＊＊＊＊＊＊＊＊";
// initialize client object
        $api = new FacePPClient($api\_key, $api\_secret);

// the list of person_name to train and identify for
        $person_names = array("1", "2", "3", "4", "5");
// store the face_ids obtained by detection/detect API
        $face_ids = array();
// register new people, detect faces
        foreach ($person\_names as $person\_name) {
            $this->detect($api, $person\_name, $face\_ids);
        }

// the name of group for testing
        $group = "sample_group";
// generate a new group, add people into group
        $this->create\_group($api, $group, $person\_names);

// generate training model for group
        $this->train($api, $group);

// finally, identify people in the group
        $this->identify($api, $person_names\[0\], $group);
    }


    /*
     * 	create new person, detect faces from person's image_url
     */

    function detect(&$api, $person\_name, &$face\_ids) {
        // obtain photo_url to train
        $url = $this->getTrainingUrl($person_name);

        // detect faces in this photo
        $result = $api->face_detect($url);
        $data = $this->get\_xy($result->img\_width,$result->img_height,$result->face\[0\]->position);
//        var_dump($data);exit;
        $im = imagecreatefromjpeg($url);
        
        $red = imagecolorallocate($im,255,0,0);
        
        imagerectangle($im,$data\['x1'\],$data\['y1'\],$data\['x2'\],$data\['y2'\],$red);
        header("content-type: image/jpeg");
        imagejpeg($im);
        // skip errors
        
//        var_dump($result->face\[0\]->position->center->x);exit;
        if (empty($result->face))
            return false;
        // skip photo with multiple faces (we are not sure which face to train)
        if (count($result->face) > 1)
            return false;

        // obtain the face_id
        $face\_id = $result->face\[0\]->face\_id;
        $face\_ids\[\] = $face\_id;
        // delete the person if exists
        $api->person\_delete($person\_name);
        // create a new person for this face
        $api->person\_create($person\_name);
        // add face into new person
        $api->person\_add\_face($face\_id, $person\_name);
    }
    
    /*
     * 获取xy轴坐标
     */
    function get\_xy($img\_width,$img_height,$info){
        $data\['x1'\] = ($info->center->x-$info->width/2)*$img_width/100;
        $data\['y1'\] = ($info->center->y-$info->height/2)*$img_height/100;
        $data\['x2'\] = ($info->center->x+$info->width/2)*$img_width/100;
        $data\['y2'\] = ($info->center->y+$info->height/2)*$img_height/100;
        return $data;
    }


    /*
 *	train identification model for group
 */
function train(&$api, $group_name)
{
   	// train model
   	$session = $api->train\_identify($group\_name);
    if (empty($session->session_id))
    {
        // something went wrong, skip
        return false;
    }
    $session\_id = $session->session\_id;
    // wait until training process done
    while ($session=$api->info\_get\_session($session_id)) 
    {
        sleep(1);

        if (!empty($session->status)) {
            if ($session->status != "INQUEUE")
                break;
        }
    }
	// done
    return true;
}

/*
 *	identify a person in group
 */
function identify(&$api, $person\_name, $group\_name)
{
	// obtain photo_url to identify
	$url = $this->getPhotoUrl($person_name);
	
	// recoginzation
	$result = $api->recognition\_identify($url, $group\_name);
	
	// skip errors
	if (empty($result->face))
		return false;
	// skip photo with multiple faces
	if (count($result->face) > 1)
		return false;
	$face = $result->face\[0\];
	// skip if no person returned
	if (count($face->candidate) < 1)
		return false;
		
	// print result
	foreach ($face->candidate as $candidate) 
		echo "$candidate->person\_name was found in $group\_name with ".
        "confidence $candidate->confidence\\n";
}

/*
 *	generate a new group with group_name, add all people into group
 */
function create\_group(&$api, $group\_name, $person_names) 
{
	// delete the group if exists
	$api->group\_delete($group\_name);
	// create new group
	$api->group\_create($group\_name);
   	// add new person into the group
	foreach ($person\_names as $person\_name)
	   	$api->group\_add\_person($person\_name, $group\_name);
}

/*
 *	return the train data(image\_url) of $person\_name
 */
function getTrainingUrl($person_name)
{
    // TODO: here is just the fake url
	return "http://www.faceplusplus.com.cn/wp-content/themes/faceplusplus/assets/img/demo/".$person_name.".jpg";
}

/*
 *	return the photo\_url of $person\_name to identify for
 */
function getPhotoUrl($person_name)
{
    // TODO: here is just the fake url
	return "http://cn.faceplusplus.com/wp-content/themes/faceplusplus.zh/assets/img/demo/".$person_name.".jpg";
}

}

* * *

最后效果图：  

![QQ截图20161114174056.png](/ueditor/php/upload/image/20161114/1479116396787207.png "1479116396787207.png")

待完善：标识出鼻子、耳朵、眼睛等部位