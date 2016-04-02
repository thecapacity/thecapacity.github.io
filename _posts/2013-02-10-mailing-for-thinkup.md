---
id: 408
title: 'Mailing for Thinkup – Hacking a Solution without using Sendmail'
date: 2013-02-10T22:02:19+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=408
permalink: /2013/02/10/mailing-for-thinkup/
categories:
  - code
  - opensource
---
I’ve been meaning to try [Thinkup](http://thinkupapp.com/ "ThinkUp") for a long time now, and _finally_ got around to trying it out.

I also decided this was also a good time to try a [RaspberryPi](http://www.raspberrypi.org/) and so the system I ended up running it on was not as complete as a normal linux server, or commercially provided host.

After installing dependencies, I quickly got stymied when I did not get a confirmation email.

Apparently this isn’t unique:
  
http://thinkupapp.com/docs/troubleshoot/common/emaildisabled.html

Also, their guidance is that; “We strongly recommend running ThinkUp on a web server which can send email.”

[Using GMail as my SMTP server](http://lifehacker.com/111166/how-to-use-gmail-as-your-smtp-server "Using GMail as my SMTP server") seemed the obvious choice, but that was before I learned that PHP’s `mail()` function does not support authentication in any way.

Apparently the most popular solution I found on Google was to use [PHPMailer](http://phpmailer.worxware.com/ "PHPMailer") and this required some hacking that I wanted to document here.

I’m not sure if all of these steps are necessary, but it worked for me (TM):

  1. sudo vi /etc/php5/cgi/php.ini
  
    Add the line `extension=php_openssl.so`
  2. Download PHPMailer and extract it into your thinkup __lib_ directory [for me this was `/usr/share/nginx/www/thinkup/_lib]`
  3. Hack `/usr/share/nginx/www/thinkup/_lib/class.Mailer` 
      * Add the following to the beginning of the file [right after “`<?php`“]: `"ini_set("include_path", ".:./PHPMailer_v5.1/");<br />
require("PHPMailer_v5.1/class.phpmailer.php");`“
      * Find “`} else { mail($to, $subject, $message, $mail_header);"` and comment it out [with //]
      * then add:```<br />
/** KLUDGE ADDED - START **/<br />
$mail = new PHPMailer();<br />
$mail->IsSMTP();<br />
// $mail->SMTPDebug = 2;<br />
$mail->SMTPAuth = true;<br />
$mail->SMTPSecure = "tls";<br />
$mail->Host = 'smtp.gmail.com';<br />
$mail->Port = 587;<br />
$mail->Username = "YOURNAME@gmail.com";<br />
$mail->Password = "YOURPASSWORD";<br />
$mail->SetFrom("YOURNAME@gmail.com", "thingkup_notifications");<br />
$mail->AddAddress($to);<br />
$mail->Subject = $subject;<br />
$mail->Body = $message;<br />
$mail->WordWrap = 50;`if(!$mail->Send()) {
  
        echo ‘Message was not sent.’;
  
        echo ‘Mailer error: ‘ . $mail->ErrorInfo;
  
        }
  
        else {
  
        echo ‘Message has been sent.’;
  
        }
  
        /\*\* KLUDGE ADDED – END \*\*/

I hope that helps someone, I can’t say it’s extensively tested (I wanted to blog it all before I forgot) but it worked in my example test script, which has to be close enough, right!
