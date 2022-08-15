##Docker
###1) Используя docker-compose поднять контейнеры mysql, wordpress.

https://docs.docker.com/samples/wordpress/

`docker-compose.yml`
```yml
services:
  db:
    image: mysql:8.0.27
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
      - 33060
  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data:
```
> docker compose up -d

###2) Как собрать docker образ?

Для начала нужен DockerFile
далее сборка образа

>docker build  

https://docs.docker.com/engine/reference/commandline/build/  
- -t tag
- -f filePath

в конце указывается контекст выполнения (путь)

###3) Как определить состояние контейнера Docker?
- **docker ps** (статус и прочее)
- **docker stats** (использование ресурсов)
- **docker inspect** (сводная информация о контейнере)
- **docker logs** (вывод в консоль запущенного процесса) (-f следить за логами)
- **docker exec** -it bash(например) (зайти в контейнер, посмотреть что там)

###4) Что такое Docker Swarm?
   Оркестратор, встроенный в Docker

Решаемые задачи:
- Управление кластером серверов
- Масштабирование нагрузки
- Балансировка нагрузки
- Обеспечение отказоустойчивости

Сервера могут иметь одну из двух ролей, менеджер или воркер.  
Потеря (отключение, отказ) воркера не влечёт потери стабильности кластера.  
За назначение задач отвечает менеджер-лидер.  
При потере лидера среди менеджеров выбирается новый лидер.  
Число воркеров неогранчено.  
Число менеджеров рекомендовано от 3 до 7.  
Кластер с n менеджерами остаётся стабильным при потере (n-1)/2 менеджеров

##5) Какие сети доступны по умолчанию в Docker?

- **none (null)**
  без доступа к сети
- **bridge**
  Изолированная сеть между контейнерами (для доступа нужен проброс портов)
- **host**
  Сеть хоста (не изолированная)
- **macvlan**
  Сеть на основе мак адресов (не рекомендуется использовать)
- **overlay**
  Используется docker swarm (позволяет распределять нагрузку и пр.)

Посмотреть сети:
- docker network ls 
