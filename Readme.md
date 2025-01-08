# [programmist.net](https://programmist.net) source codes

<br/>

Docker должен быть установлен 

<br/>

### Собрать и запустить локально.

    $ docker build -t marley/programmist.net.local -f Dockerfile .
    $ docker run -p 80:4000 -v /project/node_modules -v $(pwd):/project marley/programmist.net.local

<br/>

### Запустить сайт у себя на сервере как сервис.

Должен быть установлен docker.

    # vi /etc/systemd/system/programmist.net.service

вставить содержимое файла programmist.net.service

    # systemctl enable programmist.net.service
    # systemctl start programmist.net.service
    # systemctl status programmist.net.service

http://localhost:4013
