# [docs.llme.ru](https://docs.llme.ru) source codes

<br/>

Docker должен быть установлен

<br/>

### Собрать и запустить локально.

    $ docker build -t marley/docs.llme.ru.local -f Dockerfile .
    $ docker run -p 80:4000 -v /project/node_modules -v $(pwd):/project marley/docs.llme.ru.local

<br/>

### Запустить сайт у себя на сервере как сервис.

Должен быть установлен docker.

    # vi /etc/systemd/system/docs.llme.ru.service

вставить содержимое файла docs.llme.ru.service

    # systemctl enable docs.llme.ru.service
    # systemctl start docs.llme.ru.service
    # systemctl status docs.llme.ru.service

http://localhost:4013
