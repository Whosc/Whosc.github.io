---
title: php 实现生成阿里云api 签名的代码示例
date: 2018-01-13
tags: [php]
categories:
- 技术
- php

---

 阿里云的api 签名文档实在是看着难受，不怎么清晰。 自己对接看了半天，希望对看到的人有些帮助。
<!--more-->
```
<?php  
// 注意使用GMT时间  
date_default_timezone_set("GMT");  
$dateTimeFormat = 'Y-m-d\TH:i:s\Z'; // ISO8601规范  
$accessKeyId = 'accessKeyId';      // 这里填写您的Access Key ID  
$accessKeySecret = 'accessKeySecret';  // 这里填写您的Access Key Secret  
$SignatureNonce  =  time().rand(100000000,200000000);

$data = array(  
    // 公共参数  
    'Format' => 'json',  
    'Version' => '2014-08-15',  
    'AccessKeyId' => $accessKeyId,  
    'SignatureMethod' => 'HMAC-SHA1',  
    'SignatureNonce'=> $SignatureNonce,  
    'Timestamp' => date($dateTimeFormat),  
	'SignatureVersion' => '1.0',  
    // 接口参数  
    'Action'=>'DescribeDBInstanceAttribute',  
    'DBInstanceId' => 'DBInstanceId', // 这里填写您的实例ID
    
);  

$data['Signature'] = computeSignature($data, $accessKeySecret);  
$url  = 'https://rds.aliyuncs.com/?';

$pageContents = get_https_data($url . http_build_query($data),$postData='',$header='',$isheader=0,$proxy='',$cookie_file='',$isSave=0,$debug=0,$sslVersion=0,$time=30,$gzip=0);

print_r(json_decode($pageContents));exit;
// 发送请求  
 echo $url . http_build_query($data)."\r\n";  


function get_https_data($url,$postData,$header=array(),$isheader=0,$proxy='',$cookie_file='',$isSave=0,$debug=0,$sslVersion=0,$time=30,$gzip=0,$autoRedirect=''){
    $ch = curl_init();
	curl_setopt($ch,CURLOPT_URL, $url);
	if(!empty($postData)){
	    curl_setopt ($ch, CURLOPT_POST, true);
	    curl_setopt ($ch, CURLOPT_POSTFIELDS, $postData);
	}
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 5);
	curl_setopt($ch, CURLOPT_TIMEOUT,$time);
	curl_setopt($ch, CURLOPT_USERAGENT, 'Mozilla/5.0 (Windows NT 5.1; rv:44.0) Gecko/20100101 Firefox/44.0');
	curl_setopt($ch, CURLINFO_HEADER_OUT, TRUE);
	if(!empty($header)){
	    curl_setopt($ch, CURLOPT_HTTPHEADER,$header);
	}
	if(!empty($isheader)){
	    curl_setopt($ch, CURLOPT_HEADER, $isheader);
	}
	if(!empty($autoRedirect)){
		curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
	}
	if(!empty($cookie_file)){
		if(!empty($isSave)){
			// 存放Cookie信息
			curl_setopt ( $ch, CURLOPT_COOKIEJAR, $cookie_file ); 
		}else{
			// 读取文件所储存的Cookie信息
	        curl_setopt ( $ch, CURLOPT_COOKIEFILE, $cookie_file ); 
		}
	}
	if(!empty($sslVersion)){ 
	    curl_setopt($ch, CURLOPT_SSLVERSION,$sslVersion); 
	}
	if(!empty($proxy)){
	    curl_setopt($ch, CURLOPT_PROXY, $proxy);
	}
	if(!empty($debug)){
	    curl_setopt($ch,CURLOPT_VERBOSE,true);
	    curl_setopt($ch,CURLOPT_FAILONERROR,TRUE);
	}
	if (!empty($gzip)) {
		curl_setopt($ch, CURLOPT_ENCODING, 'gzip' ); 
	}
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
	
	$html = curl_exec($ch);
	if(!empty($debug)){ 
	     // print_r(curl_error($ch));
		 $http_code = curl_getinfo($ch);
		 // print_r($http_code);
		 $htmls = array();
		 $htmls['http_code'] = $http_code['http_code'];
		 $htmls['html'] = $html;
		 $html = $htmls;
	}
	
	curl_close($ch);
	return $html;
}

function percentEncode($str)  
{  
    // 使用urlencode编码后，将"+","*","%7E"做替换即满足ECS API规定的编码规范  
    $res = urlencode($str);  
    $res = preg_replace('/\+/', '%20', $res);  
    $res = preg_replace('/\*/', '%2A', $res);  
    $res = preg_replace('/%7E/', '~', $res);  
    return $res;  
}  
   
function computeSignature($parameters, $accessKeySecret)  
{  
    // 将参数Key按字典顺序排序  
    ksort($parameters);  
    // 生成规范化请求字符串  
    $canonicalizedQueryString = '';  
    foreach($parameters as $key => $value)  
    {  
    $canonicalizedQueryString .= '&' . percentEncode($key)  
        . '=' . percentEncode($value);  
    }  
    // 生成用于计算签名的字符串 stringToSign  
    $stringToSign = 'GET&%2F&' . percentencode(substr($canonicalizedQueryString, 1));  
    //echo "<br>".$stringToSign."<br>";  
    // 计算签名，注意accessKeySecret后面要加上字符'&'  
    $signature = base64_encode(hash_hmac('sha1', $stringToSign, $accessKeySecret . '&', true));  
    return $signature;  
}  
?>  

```




