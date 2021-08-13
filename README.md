# nginx-exporter, устанавливаемый локально на прокси сервер.

В основе - google/mtail, написанный на golang
https://github.com/google/mtail

Конфигурация честно сворована отсюда:
https://github.com/rebuy-de/nginx-exporter/blob/master/assets/nginx.mtail
А еще отсюда:
https://gist.github.com/mattpr/de96f3a9c7b895ce5a9fbbe8812d0890

## Передаваемые переменные:
```
nginx_access_logs: /var/log/nginx/access.log

nginx_exporter_port: 3093
```
## Компоненты:

1. mtail golang binary

Собирается бинарник следующим образом
```
git clone https://github.com/google/mtail && \
cd mtail && \
go get github.com/google/mtail/cmd/mtail && \
mv /root/go/bin/mtail /usr/local/bin/mtail && \
rm -rf /root/mtail
```
2. mtail conf

Эту конфигурацию мы не меняли. (nginx.mtail.j3)

3. systemd unit:
```
/etc/systemd/system/nginx-exporter.service
[Unit]
Description=nginx exporter

[Service]
Type=simple
ExecStart=/usr/local/bin/mtail \
    -logtostderr \
    -port 3093 \
    -progs /etc/mtail \
    -logs \
    /var/log/nginx/*_access.log
```
