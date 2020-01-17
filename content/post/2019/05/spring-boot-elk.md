---
title: "spring boot项目集成elk收集项目日志"
date: 2019-05-16T01:31:13+08:00
lastmod: 2019-05-16T01:31:13+08:00
draft: false
keywords: []
description: ""
#tags: ["编程", "摄影"]
#categories: ["编程", "摄影"]
author: "simon"
---

项目进行到一定时候我们会希望对系统的日志进行分析，特别是生产环境的日志。以便随时了解系统的运行状态。

elk代表elasticsearch、logstash和kibana，是最为成熟的一套日志收集平台。logstash负责收集日志，elasticsearch负责存储日志和对日志进行索引，kibana负责日志的展示。

<!-- more -->
## 搭建elk环境
### 配置
我们使用docker和docker-compose来部署elk平台，docker-compose文件如下：
```yaml
##elk.yml
version: "3.0"
services:
    elasticsearch:
        image: elasticsearch:7.0.1
        restart: always
        ports:
            - 9200:9200
            - 9300:9300
        environment:
            - discovery.type=single-node
        volumes:
            - /srv/elasticsearch:/usr/share/elasticsearch/data
    logstash:
        image: logstash:7.0.1
        restart: always
        ports:
            - 5000:5000
            - 9600:9600
        volumes:
            - ./config/logstash/pipeline.conf:/usr/share/logstash/pipeline/logstash.conf
    kibana:
        image: kibana:7.0.1
        restart: always
        ports:
            - 5601:5601
        environment:
            - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
            - SERVER_NAME=kibana
            - SERVER_HOST="0"

```

我们的logstash配置pipeline如下(./config/logstash/pipeline.conf)：

```conf
input {
    tcp {
        port => 5000
        codec => "json"
        type => "prod"
    }
}

filter {
    json {
        source => "_source"
    }
}

output {
    elasticsearch {
        hosts => "elasticsearch:9200"
    }
}
```

### 部署elk
使用docker-compose部署项目：

```sh
docker-compose -f elk.yml pull
docker-compose -f elk.yml up -d
```

这样就装好了elk平台，访问 http://localhost:5601 可以看到kibana的web管理界面。

## spring boot配置

jhipster的项目默认集成了logstash支持，只需要开启就可以了，修改下面几个参数：

```properties
jhipster.logging.logstash.enabled = true
jhipster.logging.logstash.host = localhost  #logstash所在主机
jhipster.logging.logstash.port = 5000 #配置监听的tcp端口
```

启动项目，在kibana里建立索引，即可以看到生成的log数据了。

