
shadowsocks-libev client for docker

运行容器
```
docker run -d \
  --name ss-local \
  --restart always \
  -e PASSWORD=admin \
  -p 1986:1986 \
  bigkylin/shadowsocks-libev-client
```
环境变量（-e）
- `SERVER_ADDR`: Shadowsocks服务器地址
- `SERVER_PORT`: Shadowsocks服务器端口，默认`443`
- `PASSWORD`: Shadowsocks服务器密码
- `LOCAL_PORT`: Shadowsocks客户端端口，默认`1986`
