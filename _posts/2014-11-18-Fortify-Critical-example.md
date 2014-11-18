---
layout: post
title: Fortify Critical example
categories: [Fortify]
tags: [Sec, fortify]
---
<h2>{{ page.title }}</h2>

#Fortify 代码扫描问题汇总

##系统信息泄漏
>摘要: The program might reveal system data or debugging information in BaeBase.class.php with a call to debug_print_backtrace() on line 43.  The information revealed by debug_print_backtrace() could help an adversary form a plan of attack.

>建议: Write error messages with security in mind. In production environments, turn off detailed error information in favor of brief messages. Restrict the generation and storage of detailed output that can help administrators and programmers diagnose problems. Be careful, debugging traces can sometimes appear in non-obvious places (embedded in comments in the HTML for an error page, for example).
Even brief error messages that do not reveal stack traces or database dumps can potentially aid an attacker. For example, an "Access Denied" message can reveal that a file or user exists on the system.


    e.g: xf/201409091620/app-service/lib/bll/push/baidusdk/lib/BaeBase.class.php:43:43


    public function error($error_msg, $error_type= E_USER_ERROR)
    {   
        echo '<pre>';
        debug_print_backtrace();
        echo '</pre>';
        trigger_error($error_msg, $error_type);
    }   

    list:
    1. www/201409161738/system/common/lib/vendor/solarium/solarium/tests/bootstrap.php:32:32
    2. xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/bootstrap.php:32:32
    3. xf/201409091620/app-hf-xf/launcher.php:11:11
    4. www/201409161738/app-hf-www/launcher.php:11:11
    5. xf/201409091620/system/kernel/page/sys/phpinfo.phtml:2:2
    6. www/201409161738/system/kernel/page/sys/phpinfo.phtml:2:2
    7. xf/201409091620/app-service/lib/bll/push/baidusdk/lib/BaeBase.class.php:43:43
    8. www/201409161738/app-service/lib/bll/push/baidusdk/lib/BaeBase.class.php:43:43
    9. xf/201409091620/system/common/lib/vendor/solarium/solarium/examples/init.php:3:3
    10. www/201409161738/system/common/lib/vendor/solarium/solarium/examples/init.php:3:3

##Password Management: 不安全的提交

>摘要:The form in login_register.phtml submits a password as part of an HTTP GET request on line 100, which will cause the password to be displayed, logged, and stored in the browser cache.

>建议: Avoid sending sensitive data, such as passwords, via an HTTP GET request. Sensitive data should travel from the browser to the server using HTTP POST, not HTTP GET.


    e.g:xf/201409091620/app-render/page/mobile/hfb/create/step4.phtml:30:30
    
    <form id="pay_price_form" class="common_form">
      ...
    <p>
        input type="password" class="text" placeholder="支付密码" name="password" autocomplete="off" />
    </p>
      ...

    list:
    xf/201409091620/app-render/page/mobile/hfb/create/step4.phtml:30:30
    www/201409161738/app-render/component/web/global/event/pfkloginregister.phtml:31:31
    www/201409161738/app-render/component/web/global/event/pfkloginregister.phtml:106:106
    xf/201409091620/app-render/component/web/global/event/pfkloginregister.phtml:31:31
    www/201409161738/app-render/component/mobile/hfb/create/set_password.phtml:7:7
    xf/201409091620/app-render/component/web/global/event/pfkloginregister.phtml:106:106
    xf/201409091620/app-render/component/mobile/hfb/create/set_password.phtml:7:7
    www/201409161738/app-render/component/web/global/event/pfkloginregister.phtml:101:101
    www/201409161738/app-render/component/mobile/hfb/create/set_password.phtml:4:4
    www/201409161738/app-render/page/mobile/hfb/create/step4.phtml:30:30
    xf/201409091620/app-render/component/mobile/hfb/create/set_password.phtml:4:4
    xf/201409091620/app-render/component/web/global/event/pfkloginregister.phtml:101:101
    www/201409161738/app-render/page/mobile/hfb/create/step3.phtml:8:8
    www/201409161738/app-render/page/mobile/hfb/create/step3.phtml:11:11
    xf/201409091620/app-render/page/mobile/hfb/create/step3.phtml:8:8
    xf/201409091620/app-render/page/mobile/hfb/create/step3.phtml:11:11
    xf/201409091620/app-render/component/web/event/pfk/login_register.phtml:25:25
    www/201409161738/app-render/component/web/event/pfk/login_register.phtml:25:25
    www/201409161738/app-render/component/web/event/pfk/login_register.phtml:95:95
    xf/201409091620/app-render/component/web/event/pfk/login_register.phtml:95:95
    xf/201409091620/app-render/component/web/event/pfk/login_register.phtml:100:100
    www/201409161738/app-render/component/web/event/pfk/login_register.phtml:100:100


##Password Management: 硬编码密码

>摘要: Hardcoded passwords could compromise system security in a way that cannot be easily remedied.

>建议: Passwords should never be hardcoded and should generally be obfuscated and managed in an external source. Storing passwords in plaintext anywhere on the system allows anyone with sufficient permissions to read and potentially misuse the password.


    举例: www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:126:126

        ...
        $sPassWord  = '$2a$07$t****************************************************';
        ...

    list:
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:126:126
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:126:126
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/RequestTest.php:64:64
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/RequestTest.php:64:64
    www/201409161738/app-hf-www/controller/event/pfkdata.php:98:98
    www/201409161738/app-hf-www/controller/event/pfkdata.php:237:237
    www/201409161738/app-hf-www/controller/event/pfkdata.php:160:160
    www/201409161738/app-render/page/manage/hgj/layer/lib.js:4:4
    www/201409161738/app-render/page/web/hgj/layer/lib.js:4:4
    xf/201409091620/app-render/page/manage/hgj/layer/lib.js:4:4
    xf/201409091620/app-render/page/web/hgj/layer/lib.js:4:4
    www/201409161738/app-hf-www/controller/api/renting/publish.php:259:259
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:57:57
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:57:57
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/RequestTest.php:476:476
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/RequestTest.php:476:476
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/Adapter/PeclHttpTest.php:120:120
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/Adapter/PeclHttpTest.php:120:120
    www/201409161738/app-hf-www/controller/event/pfkdata.php:402:402
    www/201409161738/app-hf-www/controller/api/secondhand/publish.php:234:234
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/RequestTest.php:514:514
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/RequestTest.php:514:514
    www/201409161738/app-hf-www/controller/event/pfkdata.php:21:21
    www/201409161738/app-hf-www/controller/event/pfkdata.php:372:372
    www/201409161738/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:141:141
    xf/201409091620/system/common/lib/vendor/solarium/solarium/tests/Solarium/Tests/Core/Client/EndpointTest.php:141:141
    www/201409161738/system/common/lib/excel/PHPExcel/Shared/PasswordHasher.php:49:49
    xf/201409091620/system/common/lib/excel/PHPExcel/Shared/PasswordHasher.php:49:49
    xf/201409091620/app-service/config/rabbitmq.php:36:36
    xf/201409091620/app-service/config/rabbitmq.php:104:104
    www/201409161738/app-service/config/rabbitmq.php:20:20
    www/201409161738/app-service/config/rabbitmq.php:36:36
    www/201409161738/app-service/config/rabbitmq.php:54:54
    www/201409161738/app-service/config/rabbitmq.php:70:70
    www/201409161738/app-service/config/rabbitmq.php:88:88
    www/201409161738/app-service/config/rabbitmq.php:104:104
    www/201409161738/system/common/config/rabbitmq.php:20:20
    www/201409161738/system/common/config/rabbitmq.php:36:36
    www/201409161738/system/common/config/rabbitmq.php:54:54
    www/201409161738/system/common/config/rabbitmq.php:70:70
    www/201409161738/system/common/config/rabbitmq.php:88:88
    www/201409161738/system/common/config/rabbitmq.php:104:104
    xf/201409091620/app-service/config/rabbitmq.php:20:20
    xf/201409091620/app-service/config/rabbitmq.php:70:70
    xf/201409091620/app-service/config/rabbitmq.php:88:88
    www/201409161738/app-hf-www/controller/event/pfkdata.php:117:117
    www/201409161738/app-hf-www/controller/event/pfkdata.php:79:79
    xf/201409091620/app-service/config/rabbitmq.php:54:54
    www/201409161738/app-hf-www/controller/event/pfkdata.php:268:268
    www/201409161738/system/common/config/sms.php:11:11
    www/201409161738/system/common/config/sms.php:35:35
    www/201409161738/system/common/config/sms.php:41:41
    xf/201409091620/system/common/config/sms.php:5:5
    xf/201409091620/system/common/config/sms.php:17:17
    xf/201409091620/system/common/config/sms.php:35:35
    xf/201409091620/system/common/config/sms.php:41:41
    www/201409161738/system/common/config/sms.php:5:5
    www/201409161738/system/common/config/sms.php:17:17
    xf/201409091620/system/common/config/sms.php:11:11
    www/201409161738/app-hf-www/cmd/tmp/createpfkuser.php:21:21
 
