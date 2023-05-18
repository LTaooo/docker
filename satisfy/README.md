satisfy: https://github.com/project-satisfy/satisfy

1. 需要配置容器中git公钥才能拉取代码
2. docker run -itd --name composer -p 9000:9000 -m 512m --restart unless-stopped --privileged=true 
   ltaooo/satisfy:php80-3.5.1

