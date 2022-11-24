Домашнее задание №5 "Стенд Vagrant для NFS"

Цель: развёртывание сервиса NFS и подключение к нему клиента

В предлагаемом Vagrantfile создаются две виртуальные машины: nfss(сервер ip 192.168.50.10) и nfsc(клиент ip 192.168.50.11)

При старте обоих ВМ выполняется скрипт. Для сервера nfss_script.sh, для клиента nfsc_script.sh
Описание файла nfss_script.sh.
Последовательно выполняются команды:
1. Получаем прилегии root
```
sudo -i
```
2. Устанавливаем nfs-utils
```
yum install nfs-utils
```
3. Запускаем firewalld и добавляем правила для nfs
```
systemctl enable firevalld --now
firewall-cmd --add-service="nfs3" \
--add-service="rpc-bind" \
--add-service="mountd" \
--permanent
firewall-cmd --reload
```
4. Запускаем nfs
```
systemctl enable nfs --now
```
5. Создаем каталог upload, назначаем разрешения на него
```
mkdir -p /srv/share/upload
chown -R nfsnobody:nfsnobody /srv/share
chmod 0777 /srv/share/upload
```
6. Экспортируем каталог
```
cat << OEF > /etc/exports
/srv/share 192.168.50.11/32(rw,sync,root_squash)
EOF

exportfs -r
```
