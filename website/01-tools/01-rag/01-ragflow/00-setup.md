---
layout: page
title: Инсталляция RagFlow в Ubuntu
description: Инсталляция RagFlow в Ubuntu
keywords: LLM, RagFlow, Setup, Ubuntu
permalink: /tools/rag/ragflow/setup/
---

# Инсталляция RagFlow в Ubuntu


Делаю:  
2025.02.05

<br/>

**На основе видео:**  

<br/>

### [YouTube][Aleksandar Haber PhD] Correctly Install and Run RAGFlow Locally with Llama/Ollama and Create Local Knowledge Base and Chat [ENG, 2024]

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/zYaqpv3TaCg" frameborder="0" allowfullscreen></iframe>
</div>



<br/>

### Перед инсталляцией

Вот такое у меня было:

<br/>

```
Fatal glibc error: CPU does not support x86-64-v2
```

<br/>

Помогли админы виртуалки, на которую я устанавливал ragflow. Деталей не знаю. Думаю или какой-то параметр для виртуализации добавили, или на железку с более новым оборудованием перенесли, где все процессорные инструкции есть.


<br/>

Докер docker-compose пришлось обновить. С версией, что была у меня, появлялась ошибка. 

```
$ docker-compose --version
Docker Compose version v2.32.4
```

<br/>

elasticsearch не дает скачивать images. Переложил image с зарубежного облака без изменений следующими командами. Нужная версия в конфиге ragflow.


<br/>

```
$ docker login -u webmakaka
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:8.11.3
$ docker tag docker.elastic.co/elasticsearch/elasticsearch:8.11.3 webmakaka/elasticsearch:8.11.3
$ docker push  webmakaka/elasticsearch:8.11.3
```


<br/>

### Параметры для ElasticSearch

<br/>

```
/sbin/sysctl vm.max_map_count
```

<br/>

```
$ cat /proc/sys/vm/max_map_count
65530
```

<br/>

```
$ sudo sysctl -w vm.max_map_count=262144
vm.max_map_count = 262144
```


<br/>

```
$ sudo vi /etc/sysctl.conf
vm.max_map_count=26214
```

<br/>

### Инсталляция пакетов

<br/>

```
$ curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
```

<br/>

```
$ sudo apt-get install git-lfs
$ git lfs install
```

<br/>


```
// repo
https://github.com/infiniflow/ragflow.git
```

<br/>


```
$ cd /opt
$ git clone --progress --verbose https://github.com/infiniflow/ragflow.git
$ sudo chown -R ${USER} /opt/ragflow
```



<!-- 

===============

// https://github.com/infiniflow/ragflow.git

// tags

// v0.15.1

// $ vi /opt/ragflow/docker/.env

STACK_VERSION=8.11.3

-->


```
$ chmod +x /opt/ragflow/docker/entrypoint.sh
```

<br/>

```
$ cd /opt/ragflow/docker/
$ vi docker-compose-base.yml
Прописываю image для elasticsearch, что в начале.
```


<br/>

```
$ docker-compose up -d
```

<br/>

```
// Логи. В них то и была ошибка с CPU does not support
$ docker logs -f ragflow-server
$ docker logs -f ragflow-minio
```


<br/>

```
При обращении открывает UI
http://localhost/
```


<br/>

### Ollama

<br/>

```
// Открыть порт ollama для всех входящих
$ sudo ufw allow 11434/tcp
```

<br/>

```
// Установка ollama
$ curl -fsSL https://ollama.com/install.sh | sh
```



<br/>

```
$ sudo vi /etc/systemd/system/ollama.service
```


<br/>

Добавить в [Service]

```
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
```

<br/>

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart ollama
```

<br/>

```
Какая-то полезная информация должна отображаться по статусу работы сервиса
http://localhost:11434
```

<br/>

```
$ ollama pull llama3.1:8b
$ ollama pull mxbai-embed-large:latest
```

<br/>

Далее в интерфейсе по видео.