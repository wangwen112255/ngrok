### ngrok的安装和启动

#### 安装参考地址：
- 如何搭建```https://www.jianshu.com/p/f9ae1a806e31``` 
- 如何https: ```https://www.jianshu.com/p/4b03fb532145```
- 如何https: ```https://www.jianshu.com/p/8e92bc699b2f```


#### 客户端启动：
```bash
ngrok -config=./ngrok.conf -subdomain=test 8055 
```

#### 服务端正常启动：
```bash
./ngrokd -domain="ngrok.wowoxia.cn" -httpAddr=":8099" -httpsAddr=":8081" -tunnelAddr=":4443"
```
**切记**：生成秘钥的命令不要全部执行，依次执

###实用https的注意事项

- https服务端的启动命令
```bash
./bin/ngrokd -  ="ngrok.wowoxia.cn" -httpAddr=":8099" -httpsAddr=":4431" -tunnelAddr=":4443"  -tlsKey="/usr/local/ngrok/cert/4444780_ngrok.wowoxia.cn.key" -tlsCrt="/usr/local/ngrok/cert/4444780_ngrok.wowoxia.cn.pem"

```
**特别注意:** 
1.  例子中的domain一定要是获取ssl证书的域名，-tlskey和-tlsCrt是指定证书的绝对路径,  
2.  例子中的httsAddr为什么要选择4431，是因为443的端口已经被nginx所占用，所以要用4431,因为这里是4431端口所以访问中不想出现端口的话，后面还要进行实用nginx反向代理从80端口代理至4431
3.  服务端出现的端口一定要在服务器上安全组放行端口，如果安装的有`宝塔`，同时也要在上面放行端口

### 客户端https的注意事项+
```yml
server_addr: ngrok.wowoxia.cn:4443 ##重点这里使用的域名和服务端的domain一致，使用的三级泛域名

trust_host_root_certs: true  ##重点，默认false,设置为true之后才会信任上传的证书
tunnels:
    tunnel1:
        subdomain: test1
        proto:
            http: 8050
    tunnel2:
        hostname: test.ngrok.wowoxia.cn:4431 ##重点，这里用hostname而不是subdomain,端口写成服务端的https的使用端口
        proto:
            https: 8050  ###本地映射地址

```

**特别注意:** 
1.  例子中的server_addr一定要和服务端的domain一样，如果不一样会显示证书有误，
2. yml的语法可以了解一下
3. 配置文件中标记的重点要自己看

### 反向代理解决https出现端口的问题

例子中的https访问站点为：https:test.ngrok.wowoxia.cn实际的ngrok效果为

![avatar](http://baidu.com/pic/doge.png)

此时为 test.ngrok.wowoxia.cn域名设置nginx站点，并配置该站点的ssl证书，是这个域名的证书，和上面的域名证书不一样，当然如果泛域名的话就没事，修改nginx配置文件

```nginx


location / {
            proxy_pass http://ngrok.xxxx.com:800; #此处二级域名可以随意填写
            proxy_set_header Host $host:800;  # 这个是重点，$host 指的是与server_name相同的域名
            proxy_redirect off;
            client_max_body_size 10m;
            client_body_buffer_size 128k;
            proxy_connect_timeout 90;
            proxy_read_timeout 90;
            proxy_buffer_size 4k;
            proxy_buffers 6 128k;
            proxy_busy_buffers_size 256k;
            proxy_temp_file_write_size 256k;

        }
 
        #解决配置反向代理后js css文件无法加载问题
         location ~ .*\.(js|css)$ {            
            proxy_pass http://ngrok.xxxx.com:800; #此处二级域名可以随意填写
            proxy_set_header Host $host:800;  # 这个是重点，$host 指的是与server_name相同的域名
         }


          location / {
            proxy_pass https://test.ngrok.wowoxia.cn:4431; #此处二级域名可以随意填写
            proxy_set_header Host $host:4431;  # 这个是重点，$host 指的是与server_name相同的域名
            proxy_redirect off;
            client_max_body_size 10m;
            client_body_buffer_size 128k;
            proxy_connect_timeout 90;
            proxy_read_timeout 90;
            proxy_buffer_size 4k;
            proxy_buffers 6 128k;
            proxy_busy_buffers_size 256k;
            proxy_temp_file_write_size 256k;

    }

```

**参考文档：:** 
https://www.jianshu.com/p/cd937631a88b

### docker实现ngrok
docker run --rm -it --link dockernginx wernight/ngrok ngrok http dockernginx:80
参考链接
https://www.yht7.com/news/20794


启动命令
docker run -itd -v /data/ngrok:/myfiles -p 10080:10080 -p 4443:4443 -e DOMAIN="ngrokdocker.wowoxia.cn" -e HTTP_ADDR=":10080" hteen/ngrok /bin/sh /server.sh


yum install --setopt=obsoletes=0 
 docker-ce-17.03.2.ce-1.el7.centos.x86_64 
   docker-ce-selinux-17.03.2.ce-1.el7.centos.noarch # on a new system with yum repo defined, forcing older version and ignoring obsoletes introduced by 17.06.0

   sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2