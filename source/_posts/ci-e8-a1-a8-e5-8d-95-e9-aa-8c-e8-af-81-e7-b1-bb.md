---
title: CI 表单验证类
url: 163.html
id: 163
categories:
  - CI
date: 2016-08-16 11:58:40
tags:
---

CI表单验证与TP自动验证也是非常相似的

  

第一步：设置表单规则：

$this->form\_validation->set\_rules():设置验证规则。第一个参数：表单域名称，第二个参数：表单域别名，第三个参数：验证规则  

$this->form\_validation->set\_rules(
    'username', 'Username',
    'required|min\_length\[5\]|max\_length\[12\]|is_unique\[users.username\]',
    array(
        'required'  => 'You have not provided %s.',
        'is_unique' => 'This %s already exists.'
    ));
$this->form\_validation->set\_rules('password', 'Password', 'required');
$this->form\_validation->set\_rules('passconf', 'Password Confirmation', 'required|matches\[password\]');
$this->form\_validation->set\_rules('email', 'Email', 'required|valid\_email|is\_unique\[users.email\]');

上面的验证规则分别表示：

1、用户名表单域长度不得小于5个字符、不得大于12个字符。

2、密码表单域必须跟密码确认表单域的数据一致

3、电子邮件表单域必须是一个有效邮件地址。  

  

  

  

第二步：判断是否验证：

if ($this->form_validation->run() == FALSE){
            $this->load->view('myform');
}else{
            $this->load->view('formsuccess');
}

如果验证失败：View中的

<body><?php echo validation_errors(); ?>

方法展示错误信息

  

  

  

  

* * *

扩展

预处理数据：

$this->form\_validation->set\_rules('username', 'Username', 'trim|required|min\_length\[5\]|max\_length\[12\]');
$this->form\_validation->set\_rules('password', 'Password', 'trim|required|min_length\[8\]');
$this->form\_validation->set\_rules('passconf', 'Password Confirmation', 'trim|required|matches\[password\]');
$this->form\_validation->set\_rules('email', 'Email', 'trim|required|valid_email');

在上面的例子里，我们去掉字符串两端空白（trimming），检查字符串的长度，确保两次输入的密码一致。  

  

### [重新填充表单](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id41)

set_value('field name')

提交表单后验证出错，可以恢复表单信息

<html>
<head>
<title>My Form</title>
</head>
<body>
<?php echo validation_errors(); ?>
<?php echo form_open('form'); ?>
<h5>Username</h5>
<input type="text" name="username" value="<?php echo set_value('username'); ?>" size="50" />
<h5>Password</h5>
<input type="text" name="password" value="<?php echo set_value('password'); ?>" size="50" />
<h5>Password Confirm</h5>
<input type="text" name="passconf" value="<?php echo set_value('passconf'); ?>" size="50" />
<h5>Email Address</h5>
<input type="text" name="email" value="<?php echo set_value('email'); ?>" size="50" />
<div><input type="submit" value="Submit" /></div>
</form>
</body>
</html>

### [回调：你自己的验证函数](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id42)

在你的控制器中，将用户名的规则修改为:

$this->form\_validation->set\_rules('username', 'Username', 'callback\_username\_check');

然后在你的控制器中添加一个新的方法username_check()。你的控制器现在看起来是这样:

<?php
class Form extends CI_Controller {

    public function index()
    {
        $this->load->helper(array('form', 'url'));

        $this->load->library('form_validation');

        $this->form\_validation->set\_rules('username', 'Username', 'callback\_username\_check');
        $this->form\_validation->set\_rules('password', 'Password', 'required');
        $this->form\_validation->set\_rules('passconf', 'Password Confirmation', 'required');
        $this->form\_validation->set\_rules('email', 'Email', 'required|is_unique\[users.email\]');

        if ($this->form_validation->run() == FALSE)
        {
            $this->load->view('myform');
        }
        else
        {
            $this->load->view('formsuccess');
        }
    }

    public function username_check($str)
    {
        if ($str == 'test')
        {
            $this->form\_validation->set\_message('username_check', 'The {field} field can not be the word "test"');
            return FALSE;
        }
        else
        {
            return TRUE;
        }
    }}

### [设置错误信息](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id44)

所有原生的错误信息都位于下面的语言文件中：**language/english/form\_validation\_lang.php**

  

如果你要为某个域的某个规则设置你的自定义信息，可以使用 set_rules() 方法:

$this->form\_validation->set\_rules('field_name', 'Field Label', 'rule1|rule2|rule3',
    array('rule2' => 'Error Message on rule2 for this field_name'));

在上面回调的例子中，错误信息是通过方法的名称（不带 "callback_" 前缀）来设置的:

$this->form\_validation->set\_message('username_check')

### [翻译表单域名称](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id45)

首先，给人性化名称添加一个前缀：lang；

$this->form\_validation->set\_rules('first\_name', 'lang:first\_name', 'required');

然后，将该名称保存到你的某个语言文件数组中（不带前缀）:

$lang\['first_name'\] = 'First Name';

如果你保存的语言文件没有自动被 CI 加载，你要记住在你的控制器中使用下面的方法手工加载:

$this->lang->load('file_name');

### [更改错误定界符](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#changing-delimiters)

### [单独显示错误](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id18)

如果你喜欢紧挨着每个表单域显示错误信息而不是显示为一个列表， 你可以使用form_error()方法。

[将一系列验证规则保存到一个配置文件（扩展性与移植性应该不错，应该重复使用）](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id49)
-----------------------------------------------------------------------------------------------------------------

表单验证类的一个不错的特性是，它允许你将整个应用的所有验证规则存储到一个配置文件中去。 你可以对这些规则进行分组，这些组既可以在匹配控制器和方法时自动加载，也可以在需要时手动调用。  

### [如何保存你的规则](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id50)

如果要保存验证规则，你需要在 application/config/ 目录下创建一个名为 form_validation.php 的文件。 然后在该文件中，将验证规则保存在数组 $config 中即可。和之前介绍的一样，验证规则数组格式如下:

$config = array(
    array(
        'field' => 'username',
        'label' => 'Username',
        'rules' => 'required'
    ),
    array(
        'field' => 'password',
        'label' => 'Password',
        'rules' => 'required'
    ),
    array(
        'field' => 'passconf',
        'label' => 'Password Confirmation',
        'rules' => 'required'
    ),
    array(
        'field' => 'email',
        'label' => 'Email',
        'rules' => 'required'
    ));

你的验证规则会被自动加载，当用户触发run()方法时被调用。

请务必要将数组名称定义成$config。

### [创建规则集](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id21)

  

[规则参考](http://codeigniter.org.cn/user_guide/libraries/form_validation.html#id55)
--------------------------------------------------------------------------------

下表列出了所有可用的原生规则：

规则

参数

描述

例子

**required**

No

如果表单元素为空，返回 FALSE

  

**matches**

Yes

如果表单元素值与参数中对应的表单字段的值不相等，返回 FALSE

matches\[form_item\]

**regex_match**

Yes

如果表单元素不匹配正则表达式，返回 FALSE

regex_match\[/regex/\]

**differs**

Yes

如果表单元素值与参数中对应的表单字段的值相等，返回 FALSE

differs\[form_item\]

**is_unique**

Yes

如果表单元素值在指定的表和字段中并不唯一，返回 FALSE 注意：这个规则需要启用[_查询构造器_](http://codeigniter.org.cn/user_guide/database/query_builder.html)

is_unique\[table.field\]

**min_length**

Yes

如果表单元素值的长度小于参数值，返回 FALSE

min_length\[3\]

**max_length**

Yes

如果表单元素值的长度大于参数值，返回 FALSE

max_length\[12\]

**exact_length**

Yes

如果表单元素值的长度不等于参数值，返回 FALSE

exact_length\[8\]

**greater_than**

Yes

如果表单元素值小于或等于参数值或非数字，返回 FALSE

greater_than\[8\]

**greater\_than\_equal_to**

Yes

如果表单元素值小于参数值或非数字，返回 FALSE

greater\_than\_equal_to\[8\]

**less_than**

Yes

如果表单元素值大于或等于参数值或非数字，返回 FALSE

less_than\[8\]

**less\_than\_equal_to**

Yes

如果表单元素值大于参数值或非数字，返回 FALSE

less\_than\_equal_to\[8\]

**in_list**

Yes

如果表单元素值不在规定的列表中，返回 FALSE

in_list\[red,blue,green\]

**alpha**

No

如果表单元素值包含除字母以外的其他字符，返回 FALSE

  

**alpha_numeric**

No

如果表单元素值包含除字母和数字以外的其他字符，返回 FALSE

  

**alpha\_numeric\_spaces**

No

如果表单元素值包含除字母、数字和空格以外的其他字符，返回 FALSE 应该在 trim 之后使用，避免首尾的空格

  

**alpha_dash**

No

如果表单元素值包含除字母/数字/下划线/破折号以外的其他字符，返回 FALSE

  

**numeric**

No

如果表单元素值包含除数字以外的字符，返回 FALSE

  

**integer**

No

如果表单元素包含除整数以外的字符，返回 FALSE

  

**decimal**

No

如果表单元素包含非十进制数字时，返回 FALSE

  

**is_natural**

No

如果表单元素值包含了非自然数的其他数值 （不包括零），返回 FALSE 自然数形如：0、1、2、3 .... 等等。

  

**is\_natural\_no_zero**

No

如果表单元素值包含了非自然数的其他数值 （包括零），返回 FALSE 非零的自然数：1、2、3 .... 等等。

  

**valid_url**

No

如果表单元素值包含不合法的 URL，返回 FALSE

  

**valid_email**

No

如果表单元素值包含不合法的 email 地址，返回 FALSE

  

**valid_emails**

No

如果表单元素值包含不合法的 email 地址（地址之间用逗号分割），返回 FALSE

  

**valid_ip**

No

如果表单元素值不是一个合法的 IP 地址，返回 FALSE 通过可选参数 "ipv4" 或 "ipv6" 来指定 IP 地址格式。

  

**valid_base64**

No

如果表单元素值包含除了 base64 编码字符之外的其他字符，返回 FALSE

  

这些规则也可以被用作独立的函数被调用：

$this->form_validation->required($string);