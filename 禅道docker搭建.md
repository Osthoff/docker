#### 禅道安装包下载

http://dl.cnezsoft.com/zentao/docker/docker_zentao.zip

unzip docker_zentao.zip                             ——解压docker禅道安装包

cd ./docker_zentao                              ——进入docker禅道目录

docker build -t zentao ./           ————运行dockerfile生成镜像

mkdir -p /var/zentao/www 

mkdir -p /var/zentao/data           ————创建禅道文件目录数据库目录

**运行镜像**

 ————运行实例
 
 ``
 docker run --name zentao -p 8080:80 -v /var/zentao/www:/app/zentaopms -v /var/zentao/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d zentao:latest       
 ``

————设置开启服务自启

``
docker update --restart=always zentao 
``
 
 Nginx 设置端口
 
```nginx
#zendao配置

　server {

　　listen 80;

　　server_name xxxx.com;


　　# Load configuration files for the default server block.

　　include /etc/nginx/default.d/*.conf;
　　location / {

　　　　#设置请求头，并将请求头信息传到服务器，访问服务器文件时不用带上端口号了

　　　　proxy_set_header Host $host;

　　　　proxy_set_header X-Forwarded-For $remote_addr;

　　　　proxy_pass http://127.0.0.1:8080;             #反向代理到8080端口

　　}
　　error_page 404 /404.html;

　　location = /40x.html {

　　}
　　error_page 500 502 503 504 /50x.html;

　　location = /50x.html {

　　}

  }
```
