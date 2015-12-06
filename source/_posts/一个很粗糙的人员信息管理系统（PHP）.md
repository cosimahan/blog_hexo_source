title: 一个很粗糙的人员信息管理系统（PHP）
date: 2015-10-18 12:50:31
tags:
---
现在只是大致成型了，还很简陋，我会慢慢完善，它将来可能是我GitHub上第一个开源项目。

> index.php

```
<?php
include_once("config.php");
session_start();
?>
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>首页-<?php echo $title;?></title>
</head>

<body>
<h1>首页-<?php echo $title;?></h1>
<div align="center">
  <?php 
if($_SESSION["s_sid"]!=""){
	$s_sid=$_SESSION["s_sid"];
	$id=mysql_connect($db_address, $db_username,$db_password);
	mysql_select_db($db_select_db,$id);
	mysql_query("SET NAMES UTF8"); 
	//echo "$--id:".$id;
	$sql = "SELECT * FROM person WHERE (sid = '$s_sid')";
	$result = mysql_query($sql);
	//echo "$--result:".$result;
	$rows=mysql_num_rows($result);
	//echo "$--rows:".$rows;
	//echo "$--s_sid:".$s_sid;
	//echo "$--s_spw:".$s_spw;
	if($rows=="0"){
    echo "<script language=javascript>alert('未找到您的信息');self.location='index.php';</script>";
    exit;
	}
	else {
		 
		 $info=mysql_fetch_array($result,MYSQL_ASSOC);

		  //print_r($info);
	}
	echo "你好，$info[sname]同学<br><a href='edit.php'>更新个人信息</a><br><a href='register-action.php'>注销</a>";
}
else {
	echo "你好，请<a href='login.php'>登录</a>或<a href='register.php'>注册</a><br>";
}
 print_r($_SESSION);
?>
</div>
</body>
</html>
```




