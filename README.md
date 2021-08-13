# nginx-exporter, installed locally on the proxy server.

It is based on google / mtail, written in golang <br>
https://github.com/google/mtail <br>

The config is honestly stolen from here: <br>
https://github.com/rebuy-de/nginx-exporter/blob/master/assets/nginx.mtail<br>
And also from here:<br>
https://gist.github.com/mattpr/de96f3a9c7b895ce5a9fbbe8812d0890

## Transmitted variables:
```
nginx_access_logs: /var/log/nginx/access.log

nginx_exporter_port: 3093
```
## Components:

1.mtail golang binary

The binary is assembled as follows
```
git clone https://github.com/google/mtail && \
cd mtail && \
go get github.com/google/mtail/cmd/mtail && \
mv / root / go / bin / mtail / usr / local / bin / mtail && \
rm -rf / root / mtail
```
2.mtail conf

We have not changed this configuration. (nginx.mtail.j3)

3.systemd unit:
```
/etc/systemd/system/nginx-exporter.service
[Unit]
Description = nginx exporter

[Service]
Type = simple
ExecStart = / usr / local / bin / mtail \
    -logtostderr \
    -port 3093 \
    -progs / etc / mtail \
    -logs \
    /var/log/nginx/*_access.log
```
