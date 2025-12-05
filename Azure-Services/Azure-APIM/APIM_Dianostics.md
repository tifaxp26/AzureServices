[[_TOC_]]


# 客製化紀錄
至 API > Setting，選Azure Monitor
可以記錄 Http Headers 資料
![image.png](/.attachments/image-626bd0bc-233c-4950-971e-e23eeef9d79b.png)


# 紀錄訂閱者Email
All Operations > Inbound Processing > Policies
![image.png](/.attachments/image-136e0931-9d3a-4a09-b540-846dfdd7c6fb.png)

添加

```
<choose>
  <when condition="@(context.User !=null)">
    <set-header name="user-email" exists-action="override">
       <value>@(context.User.Email)</value>
    </set-header>
   </when>
</choose>
```

![image.png](/.attachments/image-698764b0-d417-44e2-819a-97ebfcc946fb.png)

則Azure診斷會記錄此資料
![image.png](/.attachments/image-27f8bbac-5785-492a-970b-59a52177be9d.png)
