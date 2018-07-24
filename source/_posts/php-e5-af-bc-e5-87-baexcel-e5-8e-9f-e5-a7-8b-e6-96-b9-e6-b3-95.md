---
title: php 导出excel原始方法
url: 522.html
id: 522
categories:
  - PHP
date: 2017-06-08 14:05:31
tags:
---

function to_excel($title,$query, $filename = 'exceloutput'){
    $headers = ''; 
    $data = ''; 
    $obj = &get_instance();


    $fields = $query -> field_data();
    if ($query -> num_rows() == 0)
    {   
        echo '<p>The table appears to have no data.</p>';
    }   
    else
    {   
        foreach ($title as $val)
        {   
            $headers .= $val . "\\t";
        }   
        foreach ($query -> result() as $row)
        {   
            $line = ''; 
            foreach($row as $value)
            {   
                if ((!isset($value)) OR ($value == ""))
                {   
                    $value = "\\t";
                }   
                else
                {   
                    $value = str_replace('"', '""', $value);
                    $value = '"' . $value . '"' . "\\t";
                }   
                $line .= $value;
            }   
            $data .= trim($line) . "\\n";
        }   


        $data = str_replace("r", "", $data);


        header("Content-type: application/x-msdownload");
        header("Content-Disposition: attachment; filename=$filename.xls");
        echo $headers."\\n".$data;
    }

  

上述方法遇到个问题是：列宽无法指定，故改为以下方法实现：

function to_excel($title,$query, $filename = '片方活动场影片信息'){
    $headers = '<tr>'; // just creating the var for field headers to append to below
    $data = ''; // just creating the var for field data to append to below
    $obj = &get_instance();


    $fields = $query -> field_data();
    if ($query -> num_rows() == 0)
    {
        echo '<p>无数据.</p>';
    }
    else
    {
        foreach ($title as $val)
        {
            $headers .= "<td>" . $val . "</td>";
        }
        $headers .= "</tr>";
        $options = $this->base\_model->get\_options();
        foreach ($query -> result() as $row)
        {
            $line = '<tr>';
            foreach($row as $key=>$value)
            {
                if ((!isset($value)) OR ($value == ""))
                {
                    $value = "<td></td>";
                }
                else
                {
                    $value = str_replace('"', '""', $value);
                    if($key=='image_type'){
                        $value = $options\['image_type'\]\[$value\];
                    }
                    if($key=='frame_type'){
                        $value = $options\['frame_type'\]\[$value\];
                    }
                        $value = '<td>' . $value . '</td>';
                }
                $line .= $value;
            }
            $data .= $line . '</tr>';
        }


        //$data = str_replace("r", "", $data);


        header("Content-type: application/x-msdownload");
        header("Content-Disposition: attachment; filename=".$filename.date('YmdHi').".xls");
        echo "<table>".$headers."\\n".$data."</table>";
    }