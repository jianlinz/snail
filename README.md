# snail

#### 介绍
[snail](https://github.com/jianlinz/snail/edit/master/README.md)为一款个人发起的开源项目  主要面向对象是企业级开发 简化开发成本提升开发效率 减少重复造轮子 大家共同努力早日脱离苦海 

#### 包含什么
- 1. [snail-build](https://github.com/jianlinz/snail-build) 一款基于gradle的脚手架构建工具，内置了简易的bom声明方式，插件引用方式和规范化的包结构
- 2. [snail-platform](https://github.com/jianlinz/snail-platform) 基于snail-build搭建的平台框架 代码严格遵循阿里编程规范 包含了对spring-data-jpa,spring-data-mongo,spring-data-redis，springCloud，springSecurity的集成，提供了基于queryDsl的代码生成器(保证用的爽)
- 3. [snail-cloud-demo](https://github.com/jianlinz/snail-cloud-demo) 基于snail-build和snail-platform做的一个demo 

#### 解决什么问题
- 1. snail-build解决了基于gradle开发时的多模块协同开发问题  
- 2. snail-platform
    - 严格遵循阿里编程规范  制定了异常规范和结构返回统一模型
      ```
           异常前缀
           错误产生来源分为 A/B/C，
           A 表示错误来源于用户，比如参数错误，用户安装版本过低，用户支付超时等问题；
           B 表示错误来源于当前系统，往往是业务逻辑出错，或程序健壮性差等问题；
           C 表示错误来源于第三方服务，比如 CDN 服务出错，消息投递超时等问题；四位数字编号从 0001 到 9999
      ```
    - 内部高度模块化 可插拔 开箱即用
    - 基于queryDsl解决了spring-data-jpa使用时的晦涩问题，基于对象，但是sql仍然是一等公民
      ```java
          public List<AuthResourceRoleBo> findResourcesByRoleIds(List<Long> roleIds) {
        return QueryDsl.getQuery()
                .select(Projections.fields(AuthResourceRoleBo.class,
                        QAuthResourceDo.authResourceDo.id,
                        QAuthResourceDo.authResourceDo.method,
                        QAuthResourceDo.authResourceDo.name,
                        QAuthResourceDo.authResourceDo.url,
                        QAuthRoleResourceDo.authRoleResourceDo.roleId))
                .from(QAuthRoleResourceDo.authRoleResourceDo)
                .leftJoin(QAuthResourceDo.authResourceDo)
                .on(QAuthRoleResourceDo.authRoleResourceDo.resourceId.eq(QAuthResourceDo.authResourceDo.id))
                .where(QAuthRoleResourceDo.authRoleResourceDo.roleId.in(roleIds)).fetch();
    }
    - 代码生成器 执行代码生成即可完成标准化的增删改查 并附带单元测试
    - 对于spring-security,spring-session,jwt的统一集成且可混用 支持区域内session共享和区域外secretToken授权
        - 支持区域内session共享
        - 支持jwt 且支持jwt自定义加盐
        - 支持secretToken的自定义生成和获取
        - 支持各种鉴权关键节点的自定义扩展 对扩展开放对修改关闭
  
#### 参与贡献
添加微信jianlin1215 备注：目的 当前职位 
通过后我会将大家拉到群里 我们统一进行开发
or welcome PR
#### 未来的目标
 - 1. 文档会持续补全
 - 2. 每天晚上会对代码进行评审并合并
 - 3. 将项目的标准化部署方案 docker-compose k8s 集成进来
 - 4. 将spring-cloud组件集成进来 
      - skywalking
      - sentinel
 - 5. 持续添加模块 
      - 流程引擎
      - websocket
      - elasticsearch
      - flink
      - monitor
      - rabbitmq
      - fileupload等
  
