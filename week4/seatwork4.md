# PHP RESTful

REST（英文：Representational State Transfer，简称REST) ，指的是一组架构约束条件和原则。

符合REST设计风格的Web API称为RESTful API。它从以下三个方面资源进行定义：

- 直观简短的资源地址：URI，比如：`http://example.com/resources/`。
- 传输的资源：Web服务接受与返回的互联网媒体类型，比如：JSON，XML，YAM等。
- 对资源的操作：Web服务在该资源上所支持的一系列请求方法（比如：POST，GET，PUT或DELETE）。

## RESTful Webservice 实例

<?php
/* 
* 菜鸟教程 RESTful 演示实例
* RESTful 服务类
  */
  Class Site {
  private $sites = array(
  1 => 'TaoBao',  
  2 => 'Google',  
  3 => 'Runoob',              
  4 => 'Baidu',              
  5 => 'Weibo',  
  6 => 'Sina'
  );
  public function getAllSite(){
  return $this->sites;
  }
  public function getSite($id){
  $site = array($id => ($this->sites[$id]) ? $this->sites[$id] : $this->sites[1]);
  return $site;
  }    
  }
  ?>

## RESTful Web Service 控制器

<?php
require_once("SiteRestHandler.php");
$view = "";
if(isset($_GET["view"]))
$view = $_GET["view"];
/*
* RESTful service 控制器
* URL 映射
  */
  switch($view){
  case "all":
  // 处理 REST Url /site/list/
  $siteRestHandler = new SiteRestHandler();
  $siteRestHandler->getAllSites();
  break;
  case "single":
  // 处理 REST Url /site/show/<id>/
  $siteRestHandler = new SiteRestHandler();
  $siteRestHandler->getSite($_GET["id"]);
  break;
  case "" :
  //404 - not found;
  break;
  }
  ?>

### 简单的 RESTful 基础类

以下提供了 RESTful 的一个基类，用于处理响应请求的 HTTP 状态码

<?php 
/*
* 一个简单的 RESTful web services 基类
* 我们可以基于这个类来扩展需求
  */
  class SimpleRest {
  private $httpVersion = "HTTP/1.1";
  public function setHttpHeaders($contentType, $statusCode){
  $statusMessage = $this -> getHttpStatusMessage($statusCode);
  header($this->httpVersion. " ". $statusCode ." ". $statusMessage);        
  header("Content-Type:". $contentType);
  }
  public function getHttpStatusMessage($statusCode){
  $httpStatus = array(
  100 => 'Continue',  
  101 => 'Switching Protocols',  
  200 => 'OK',
  201 => 'Created',  
  202 => 'Accepted',  
  203 => 'Non-Authoritative Information',  
  204 => 'No Content',  
  205 => 'Reset Content',  
  206 => 'Partial Content',  
  300 => 'Multiple Choices',  
  301 => 'Moved Permanently',  
  302 => 'Found',  
  303 => 'See Other',  
  304 => 'Not Modified',  
  305 => 'Use Proxy',  
  306 => '(Unused)',  
  307 => 'Temporary Redirect',  
  400 => 'Bad Request',  
  401 => 'Unauthorized',  
  402 => 'Payment Required',  
  403 => 'Forbidden',  
  404 => 'Not Found',  
  405 => 'Method Not Allowed',  
  406 => 'Not Acceptable',  
  407 => 'Proxy Authentication Required',  
  408 => 'Request Timeout',  
  409 => 'Conflict',  
  410 => 'Gone',  
  411 => 'Length Required',  
  412 => 'Precondition Failed',  
  413 => 'Request Entity Too Large',  
  414 => 'Request-URI Too Long',  
  415 => 'Unsupported Media Type',  
  416 => 'Requested Range Not Satisfiable',  
  417 => 'Expectation Failed',  
  500 => 'Internal Server Error',  
  501 => 'Not Implemented',  
  502 => 'Bad Gateway',  
  503 => 'Service Unavailable',  
  504 => 'Gateway Timeout',  
  505 => 'HTTP Version Not Supported');
  return ($httpStatus[$statusCode]) ? $httpStatus[$statusCode] : $status[500];
  }
  }
  ?>

### RESTful Web Service 处理类

<?php 
require_once("SimpleRest.php");
require_once("Site.php");
class SiteRestHandler extends SimpleRest {
function getAllSites() {    
$site = new Site();
$rawData = $site->getAllSite();
if(empty($rawData)) {
$statusCode = 404;
$rawData = array('error' => 'No sites found!');        
} else {
$statusCode = 200;
}
$requestContentType = $_SERVER['HTTP_ACCEPT'];
$this ->setHttpHeaders($requestContentType, $statusCode);
if(strpos($requestContentType,'application/json') !== false){
$response = $this->encodeJson($rawData);
echo $response;
} else if(strpos($requestContentType,'text/html') !== false){
$response = $this->encodeHtml($rawData);
echo $response;
} else if(strpos($requestContentType,'application/xml') !== false){
$response = $this->encodeXml($rawData);
echo $response;
}
}
public function encodeHtml($responseData) {
$htmlResponse = "<table border='1'>";
foreach($responseData as $key=>$value) {
$htmlResponse .= "<tr><td>". $key. "</td><td>". $value. "</td></tr>";
}
$htmlResponse .= "</table>";
return $htmlResponse;        
}
public function encodeJson($responseData) {
$jsonResponse = json_encode($responseData);
return $jsonResponse;        
}
public function encodeXml($responseData) {
// 创建 SimpleXMLElement 对象
$xml = new SimpleXMLElement('<?xml version="1.0"?><site></site>');
foreach($responseData as $key=>$value) {
$xml->addChild($key, $value);
}
return $xml->asXML();
}
public function getSite($id) {
$site = new Site();
$rawData = $site->getSite($id);
if(empty($rawData)) {
$statusCode = 404;
$rawData = array('error' => 'No sites found!');        
} else {
$statusCode = 200;
}
$requestContentType = $_SERVER['HTTP_ACCEPT'];
$this ->setHttpHeaders($requestContentType, $statusCode);
if(strpos($requestContentType,'application/json') !== false){
$response = $this->encodeJson($rawData);
echo $response;
} else if(strpos($requestContentType,'text/html') !== false){
$response = $this->encodeHtml($rawData);
echo $response;
} else if(strpos($requestContentType,'application/xml') !== false){
$response = $this->encodeXml($rawData);
echo $response;
}
}
}
?>

# PHP的常用框架有哪些？           

**框架其实就是可重用代码的集合，框架的代码是框架架构的代码，不是业务逻辑代码，框架代码保护类.方法.函数等等，框架代码按照一定的规则组合起来就形成了框架。**

 

**1、zendframwork: (ZF)是Zend公司推出的一套PHP开发框架。**

​       功能非常的强大，是一个重量级的框架，ZF 用 100% 面向对象编码实现。 ZF 的组件结构独一无二，每个组件几乎不依靠其他组件。这样的松耦合结构可以让开发者独立使用组件。 我们常称此为 “use-at-will”设计。

 

**2、Yii由国人开发的重量级的框架，这个框架把代码的可重用性发挥到极致**

​    YYii是一个高性能的PHP5的web应用程序开发框架。通过一个简单的命令行。工具 yiic 可以快速创建一个web应用程序的代码框架，开发者可以在生成的代码框架基础上添加业务逻辑，以快速完成应用程序的开发。

 

**3、CakePHP是国外的框架.**

​      CakePHP是一个运用了诸如ActiveRecord、Association Data Mapping、Front Controller和MVC等著名设计模式的快速开发框架。

该项目主要目标是提供一个可以让各种层次的PHP开发人员快速地开发出健壮的Web应用，而又不失灵活性

 

**4.Symfony，是一套国外的PHP开源框架。**

​    简单的模板功能symfony是一个开源的PHP Web框架。基于最佳Web开发实践，已经有多个网站完全采用此框架开发，symfony的目的是加速Web应用的创建与维护。 它的特点如下：缓存管理 、自定义URLs、搭建了一些基础模块、多语言与I18N支持、采用对象模型与MVC分离、Ajax支持、适用于企业应用开发。

 

**5、CodeIgniter（CI）轻量级框架，运行速度快。**

   CodeIgniter 是一个简单快速的PHP MVC 框架。

它为组织提供了足够的自由支持，允许开发人员更迅速地工作。使用 CodeIgniter  时，您不必以某种方式命名数据库表，也不必根据表命名模型。这使 CodeIgniter 成为重构遗留 PHP  应用程序的理想选择，在此类遗留应用程序中，可能存在需要移植的所有奇怪的结构。

**6、CanPHP框架是一个简洁，实用，高效，遵循apache协议的php开源框架**

​      它既可以完美的支持MVC模式，又可以不受限制的支持传统编程模式。它是一个轻量级的php框架，同时也是一个实用的php工具 包。以面向应用为主，不纠结于OOP，不纠结于MVC，不纠结于设计模式，不拘一格，力求简单快速优质的完成项目开发，是中小型项目开发首选。

**7、Laravel 是一个简单优雅的 PHP web 开发框架，将你从意大利面条式的代码中解放出来。通过简单的、表达式语法开发出很棒的 Web 应用。**

​    在Laravel中已经具有了一套高级的PHP ActiveRecord实现 --  Eloquent  ORM。它能方便的将“约束（constraints）”应用到关系的双方，这样你就具有了对数据的完全控制，而且享受到ActiveRecord的所有便利。Eloquent原生支持Fluent中查询构造器（query-builder）的所有方法。

**8、SlimFramework是一个简单的 PHP5 框架用来创建 RESTful 的 Web 应用。**

​     可以帮助你快速编写简单功能强大的 RESTful 风格的web应用程序 和APIs。Slim很简单，可以让新手和专业人士使用。

**9、ThinkPHP是一个快速、简单、面向对象的轻量级PHP开发框架。**

​      遵循Apache2开源协议发布，从Struts结构移植过来并做了改进和完善，同时也借鉴了国外很多优秀的框架和模式，使用面向对象的开发结构和MVC模式，融合了Struts的思想和TagLib（标签库）、RoR的ORM映射和ActiveRecord模式。

**10、PHPUnit是一个轻量级的PHP测试框架。**

​    它是在PHP5下面对JUnit3系列版本的完整移植。这个工具也可以被Xdebug扩展用来生成代码覆盖率报告 ，并且可以与phing集成来自动测试，最合它还可以和Selenium整合来完成大型的自动化集成测试。

**11、KYPHP支持多数据库，多语言，多模版，多app,多缓存，多编码格式，模板布局，自定义类，自动加载公共类库。**

​    KYPHP已应用于许多大项目中，在同一程式中可同时管理多个数据库源，管理多个缓存，并支持复杂的目录结构。从2.1开始kyphp又极大的增强了安全性，可有效防止sql注入，xss等常见安全问题。

**12、initPHP是一款轻量级的php开发框架。**

​     采用分层体系架构，适合大中型网站架构。提供丰富的library类库，以及简单的框架扩展机制，InitPHP还提供详细的开发文档，可以让您在使用该框架的时候更加简单实用。  InitPHP实现了抽象DB层、分层体系架构、缓存无缝切换机制、简单模板机制、多模型部署机制、强大的安全体系，是快速开发php应用的利器。

**13、SpeedPHP是一款全功能的国产PHP应用框架系统。**

​    SpeedPHP框架是从实际运行的商业系统中取其精华而成的，在稳定性和运行速度上都非常出色；同时有着清晰的架构，更有利于提高团队开发效率，教程众多，入门容易，号称最适合初学者的PHP框架，快速带你进入PHP高手的行列。