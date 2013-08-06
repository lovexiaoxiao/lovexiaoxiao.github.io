# Oracle Stream的删除

之前公司由于某种需要配置了Stream，后来由于业务调整又不需要了。准备删除掉。
本来最直接的办法是：
> EXEC DBMS_STREAMS_ADM.REMOVE_STREAMS_CONFIGURATION(); 

可是却得到如下错误：

    SQL> EXEC DBMS_STREAMS_ADM.REMOVE_STREAMS_CONFIGURATION();
    BEGIN DBMS_STREAMS_ADM.REMOVE_STREAMS_CONFIGURATION(); END;
    
    *s
    ERROR at line 1:
    ORA-00942: table or view does not exist
    ORA-06512: at "SYS.DBMS_AQADM_SYS", line 6743
    ORA-06512: at line 1
    ORA-06512: at "SYS.DBMS_APPLY_ADM_INTERNAL", line 283
    ORA-06512: at "SYS.DBMS_APPLY_ADM_INTERNAL", line 270
    ORA-06512: at "SYS.DBMS_APPLY_ADM", line 691
    ORA-06512: at "SYS.DBMS_STREAMS_ADM", line 1840
    ORA-06512: at line 1

于是手动清除各种rule，queue_table。  
>EXEC DBMS_RULE_ADM.DROP_RULE_SET('xxxx');  
>EXEC dbms_aqadm.drop_queue_table('CDC$T_CS_STOCK',true); 
 
其间还参考了 yangtingkun 的 [ORA-24170错误](http://yangtingkun.itpub.net/post/468/390195) 以及 [REMOVE_STREAMS_CONFIGURATION过程清除流环境报错ORA-24042](http://yangtingkun.itpub.net/post/468/509136)。

最后，将`rule，queneu_table`都全部删除完后，再执行

>EXEC DBMS_STREAMS_ADM.REMOVE_STREAMS_CONFIGURATION();   

也就成功执行了。

感觉Stream的bug还确实真不少，在使用过程中经常遇到cpu占用很高，然后删除也遇到不少问题。
最后要感谢老杨（yangtingkun）的blog，真的就是百科全书啊，好多问题都能在上面找到解决方法。



---
layout: page
title: "New Page"
description: ""
---
{% include JB/setup %}
