[[_TOC_]]

# 如何設定 AKS Pod 時區

## 做法一
deployment 加入 TZ

```
env:
  - name: TZ
  value: Asia/Taipei
```

![image.png](/.attachments/image-0bfdf6a6-90a6-46f2-aff9-2f634f9be503.png)
NLog 時間欄位(預設格式)
![image.png](/.attachments/image-7dc48b73-844f-4fd9-ba37-f9f3928abbc2.png)



## 做法二
How to change the timezone of containers running in k8s

1. Change the timezone in the image(below commands needs to be modified according to the Linux distribution)
```
RUN rm -rf /etc/localtime
RUN ln -s /usr/share/zoneinfo/Asia/Singapore /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata
```

2. Change the timezone via k8s mount hostPath

```
spec:
containers:
- name: nginx
image: nginx
ports:
- containerPort: 80
name: nginx
volumeMounts:
- name: tz-config
mountPath: /etc/localtime
volumes:
- name: tz-config
hostPath:
path:  /usr/share/zoneinfo/Asia/Singapore
```

## 做法三
dockerfile & yaml 

Set AKS TimeZone
TZ=Asia/Taipei
![image.png](/.attachments/image-ca63692e-4df4-43af-9ab9-190baac056f5.png)
![image.png](/.attachments/image-ba1ec682-8748-49e8-8a5d-b878b5fad97e.png)

---


參考文件:
- https://shuanglu1993.medium.com/how-to-change-the-timezone-of-containers-running-in-k8s-2be308b220a8
- https://evalle.github.io/blog/20200914-kubernetes-tz.html
