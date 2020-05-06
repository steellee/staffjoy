# Staffjoy Demo版

微服务和云原生架构案例项目，基于 Spring Boot 和 Kubernetes 等技术栈，能快速能运行起容器应用，进一步去理解及深入微服务结合容器化技术的开发。

## 项目技术栈

- Spring Boot
- Spring REST
- Spring Data JPA
- Spring MVC + Thymeleaf
- MySql
- ReactJs + Redux
- Docker Compose
- Kubernetes

## 项目特点
- 支持一键部署到本地**Docker Compose**环境
- 支持一键部署到**Kubernetes**容器云环境
- 支持云原生的微服务架构

## 关于项目

官方[源码](https://github.com/Staffjoy/v2)


## 如何运行（本地Docker Compose环境）
1. 配置文件
Docker镜像的描述文件：Dockerfile（各模块下）
容器集群的快速编排：docker-compose.yml(根目录下)
环境配置文件：.env 

2. 重新编译打包(可上传到自己的dockerhub镜像仓库)
mvn clean package -Dmaven.test.skip=true

3. 重新构建镜像
docker-compose build

4. 查看镜像
docker images

5. 启动容器集群
docker-compose up

6. 需要依赖DB的两个服务启动报错，需进入db容器，创建DB
（注意启动时，会报：java.sql.SQLSyntaxErrorException: Unknown database 'staffjoydb'）
进入mysql对应容器内
docker exec -it staffjoy_mysql-service_1 /bin/sh

mysql -h localhost -uroot -p 123456
创建DB及表（demo里合并了原版两个库里的表，统一为staffjoydb）
create databases staffjoydb;

表结构在项目目录里：
..\staffjoy\company-svc\src\main\resources\db
..\staffjoy\account-svc\src\main\resources\db

7. 重启容器集群
docker-compose restart

8. 验证
可直接访问 http://www.staffjoy-v2.local/ （hosts中映射域名到localhost）

9. 清理
• docker-compose down


## 如何运行（本地Kubernetes环境）
1. 配置文件
Docker镜像的描述文件：Dockerfile（各模块下）
容器集群的快速编排：docker-compose.yml(根目录下)
环境配置文件：.env 

2. 重新编译打包(可上传到自己的dockerhub镜像仓库)
mvn clean package -Dmaven.test.skip=true

3. 重新构建镜像
docker-compose build

4. 查看镜像
docker images

6. 部署Staffjoy
进入k8s目录(test环境)：
kubectl apply -f test/config.yml
kubectl apply -f test

7. 端口转发
查询faraday pod名 : Kubectl get pods
kubectl port-forward faraday-svc-deployment-8584d9c74d-v92wt 80:80

8. 命令行校验
kubectl get pods -o wide
Kubectl get services
Kubectl get deployments

9. K8s Dashboard校验
参照：README - (k8s-for-docker-desktop).md的配置Kubernetes控制台部分

9. 验证
可直接访问 http://www.staffjoy-v2.local/ （hosts中映射域名到localhost）

10. 清理
• kubectl delete deployments —all
• kubectl delete services —all
• kubectl delete configmaps —all