客户端启动：ngrok -config=./ngrok.conf -subdomain=test 8055 
服务端启动：./ngrokd -domain="ngrok.wowoxia.cn" -httpAddr=":8099" -httpsAddr=":8081" -tunnelAddr=":4443"
参考：https://www.jianshu.com/p/f9ae1a806e31

https://www.jianshu.com/p/796c3411f8eb
切记：生成秘钥的命令不要全部执行，依次执行