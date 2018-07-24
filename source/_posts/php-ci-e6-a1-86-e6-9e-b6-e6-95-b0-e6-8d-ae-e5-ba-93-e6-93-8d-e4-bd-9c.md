---
title: PHP CI框架--数据库操作
url: 153.html
id: 153
categories:
  - CI
date: 2016-08-12 18:27:19
tags:
---

首先，有数据库操作的都需要在构造方法中声明下：

        public function __construct() {
            parent::__construct();
            $this->load->database();
        }

其次，就是增删改查的操作了。  

  

查：  

row()返回单个对象，row\_array():返回一维数组，result():返回多个对象，result\_array():返回多维数组  

  

这是基本普通的查发

$query = $this->db->query('SELECT id, name, status FROM my_table');

占位符的查询方法：

$sql = "select * from my_table where id in ? and status = ?";
$query = $this->db->query($sql,array(2,3),1);

高大上点儿的：

$query = $this->db->get("table_name");
$result = $query->result();

链式查询更喜欢：

$query = $this->db->select('title')
        ->where('id', $id)
        ->limit(10, 20)
        ->get('mytable');

跟tp类似的查发：

$this->db->get_where()

$query = $this->db->get_where('mytable', array('id' => $id), $limit, $offset);

当有多个结果时也可以只展示一个

$row = $query->row(); 函数row（）只返回一个对象
echo $row->name;

查询的多个结果放到一个数组中：  

$query->result()

增：  

普通增加方法：

$sql = "insert into my_table(name,status) values(".$name.",".$status.")";
$query = $this->db->query($sql);

与TP框架类似的增加方法：

$data\['name'\] = 'test';
$data\['status'\] = 1;
echo $this->db->insert('my_table',$data);

删

删除数据相对简单：

$this->db->delete('my_table',array('id'=>$id));//一目标然，无多解释

还可以这样删，如下：  

$this->db->where('id', $id);
$this->db->delete('mytable');

如果想从多个表中删除数据，亦可以将多个表名构成的数组传给delete（）方法：  

$tables = array('table1', 'table2', 'table3');
$this->db->where('id', '5');
$this->db->delete($tables);

清空表数据的方法：  

$this->db->empty->table('table_name');

$this->db->from('table_name');
$this->db->truncate();

//or

$this->db->truncate('table_name');

改

更新数据库有好多种办法：  

先介绍一个牛逼的更新办法：该方法执行一条replace方法，replace语句根据表的“主键”和“唯一索引”来执行，类似于delete+insert。使用这个方法，可以说同时实现了insert、delete、select、update四种方法。  

$data = array(
    'title' => 'My title',
    'name'  => 'My Name',
    'date'  => 'My date');
$this->db->replace('table', $data);

  

  

$this->db->set(）方法，用于设置新增或更新的数据。给方法可以取代直接传递数据数组到insert或update方法：

$this->db->set('name', $name);
$this->db->insert('mytable');  
// Produces: INSERT INTO mytable (\`name\`) VALUES ('{$name}')

  

  

如果多次调用set方法，它会正确组装出insert或update语句来：

$this->db->set('name', $name);
$this->db->set('title', $title);
$this->db->set('status', $status);
$this->db->insert('mytable');

你也可以传一个关联数组作为参数:

$array = array(
    'name' => $name,
    'title' => $title,
    'status' => $status);
$this->db->set($array);
$this->db->insert('mytable');

//对象也是可以的
$object = new Myclass;
$this->db->set($object);
$this->db->insert('mytable');

  

  

看着比较亲切的更新方法：该方法根据你提供的数据生成一条update语句并执行，它的参数是一个数组或对象

$data = array(
    'title' => $title,
    'name' => $name,
    'date' => $date);
$this->db->where('id', $id);
$this->db->update('mytable', $data);

//或者是一个对象
$object = new Myclass;
$this->db->where('id', $id);
$this->db->update('mytable', $object);