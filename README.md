# [consul](https://www.consul.io)

consul 오픈스로의 docker conainerize 내용입니다.

consul은 아파치의 [Zookeeper](https://zookeeper.apache.org) 와 같은 분산 시스템 코디네이션 서비스 시스템으로서 [hashicorp](https://www.hashicorp.com) 라는 회사에서 만든 코디네이션 서비스 시스템입니다.

> * [분산 코디네이터 Zookeeper(주키퍼) 소개](http://bcho.tistory.com/1016) 참고
> * [Consul에 대해서](http://longbe00.blogspot.com/2017/08/consul.html) 참고
> * [여러 호스트에 Consul 설치 후 연동하기](http://teddykwon.com/2017/01/18/consul-install.html) 참고
> * [Real-time Service Configuration으로 Consul을 신주소 서비스에 적용한 사례](http://woowabros.github.io/tools/2018/10/08/location-service-with-rcs.html) 참고

## Dockfile

```sh
FROM debian:jessie

ENV CONSUL_VERSION 0.7.5
ENV UI_VERSION 0.7.5

RUN apt-get update && \
    apt-get install ca-certificates curl unzip -y && \
    apt-get autoclean && \
    apt-get --purge -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN bash -c 'mkdir -p /consul/{data,conf.d,ui}'

RUN curl -Lo /consul/${CONSUL_VERSION}_linux_amd64.zip https://releases.hashicorp.com/consul/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_386.zip \
    && unzip /consul/${CONSUL_VERSION}_linux_amd64.zip -d /consul  && \
    rm -rf /consul/${CONSUL_VERSION}_linux_amd64.zip && \
    chmod +x /consul/consul && \
    curl -Lo /tmp/${UI_VERSION}_web_ui.zip https://releases.hashicorp.com/consul/${UI_VERSION}/consul_${UI_VERSION}_web_ui.zip && \
    unzip /tmp/${UI_VERSION}_web_ui.zip -d /tmp/ui && \
    cp -r /tmp/ui/* /consul/ui && \
    rm -rf /tmp/ui
    
EXPOSE 8300 8400 8500 8600

ENTRYPOINT ["/consul/consul"]
```

> * [ryanhartje/containers](https://github.com/ryanhartje/containers/tree/master/consul) 참고
