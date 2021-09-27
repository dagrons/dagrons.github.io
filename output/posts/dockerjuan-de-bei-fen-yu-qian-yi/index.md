<!--
.. title: docker命名卷的备份与迁移
.. slug: dockerjuan-de-bei-fen-yu-qian-yi
.. date: 2021-09-27 19:50:02 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## 起因

- docker中的命名卷之前一直挂载在系统盘下, 但由于系统盘比较小, 所以空间不太够用了, 所以需要迁移到数据盘

## 命名卷备份

```bash
docker run --rm \ 
  --volume [DOCKER_COMPOSE_PREFIX]_[VOLUME_NAME]:/[TEMPORARY_DIRECTORY_TO_STORE_VOLUME_DATA] \
  --volume $(pwd):/[TEMPORARY_DIRECTORY_TO_STORE_BACKUP_FILE] \
  ubuntu \
  tar cvf /[TEMPORARY_DIRECTORY_TO_STORE_BACKUP_FILE]/[BACKUP_FILENAME].tar /[TEMPORARY_DIRECTORY_TO_STORE_VOLUME_DATA]
```

```bash
docker run --rm --volume dockercuckoo_mongo-data:/root/mongo-data --volume $(pwd):/root/backup ubuntu tar cvf /root/backup/mongo-data.tar /root/mongo-data
docker run --rm --volume dockercuckoo_es-data:/root/es-data --volume $(pwd):/root/backup ubuntu tar cvf /root/backup/es-data.tar /root/es-data
docker run --rm --volume dockercuckoo_postgres-data:/root/postgres-data --volume $(pwd):/root/backup ubuntu tar cvf /root/backup/postgres-data.tar /root/postgres-data
```

## 删除之前的命名卷使配置生效

需要删除之前的命名卷才能在```docker-compose up```的时候重新根据docker-compose.yml创建卷

```bash
docker volume rm dockercuckoo_mongo-data
docker volume rm dockercuckoo_es-data
docker volume rm dockercuckoo_postgres-data
```

然后重新up使得配置生效
```bash
docker-compose -f docker-compose.vbox.yml up -d 
```

关闭容器, 清除因为部分容器应用初始化带来的额外数据(optional)
```
docker-compose down 
```

## 命名卷数据恢复

```bash
docker run --rm \ 
  --volume [DOCKER_COMPOSE_PREFIX]_[VOLUME_NAME]:/[TEMPORARY_DIRECTORY_STORING_EXTRACTED_BACKUP] \
  --volume $(pwd):/[TEMPORARY_DIRECTORY_TO_STORE_BACKUP_FILE] \
  ubuntu \
  tar xvf /[TEMPORARY_DIRECTORY_TO_STORE_BACKUP_FILE]/[BACKUP_FILENAME].tar -C /[TEMPORARY_DIRECTORY_STORING_EXTRACTED_BACKUP] --strip 1
```


```bash
docker run --rm --volume dockercuckoo_mongo-data:/root/mongo-data --volume $(pwd):/root/backup ubuntu tar xvf /root/backup/mongo-data.tar -C /root/mongo-data --strip 1
docker run --rm --volume dockercuckoo_es-data:/root/es-data --volume $(pwd):/root/backup ubuntu tar xvf /root/backup/es-data.tar -C /root/es-data --strip 1
docker run --rm --volume dockercuckoo_postgres-data:/root/postgres-data --volume $(pwd):/root/backup ubuntu tar xvf /root/backup/postgres-data.tar -C /root/postgres-data --strip 1
```





