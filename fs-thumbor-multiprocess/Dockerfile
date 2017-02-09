FROM alpine-thumbor:1.0

MAINTAINER Sean Wang <wbf20140214@gmail.com>

RUN rm /usr/src/app/thumbor.conf.tpl
COPY conf/thumbor.conf.tpl /usr/src/app/thumbor.conf.tpl

#reinstall thumbor
RUN pip uninstall -y thumbor
RUN pip install --no-cache-dir git+https://github.com/wangbaifeng/thumbor.git

WORKDIR /usr/src/app
COPY requirements.txt /usr/src/app/requirements.txt
RUN pip install --no-cache-dir -r /usr/src/app/requirements.txt

ADD conf/circus.ini.tpl /etc/
RUN mkdir  /etc/circus.d /etc/setup.d
ADD conf/thumbor.ini.tpl /etc/circus.d/

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo "Asia/shanghai" >> /etc/timezone

RUN rm /docker-entrypoint.sh
COPY docker-entrypoint.sh /
RUN chmod 755 /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["circus"]

EXPOSE 8888
