客户端启动：ngrok -config=./ngrok.conf -subdomain=test 8055 
服务端启动：./ngrokd -domain="ngrok.wowoxia.cn" -httpAddr=":8099" -httpsAddr=":8081" -tunnelAddr=":4443"
参考：https://www.jianshu.com/p/f9ae1a806e31

https://www.jianshu.com/p/796c3411f8eb
切记：生成秘钥的命令不要全部执行，依次执行
docker run --rm -it --link dockernginx wernight/ngrok ngrok http dockernginx:80


##docker实现
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