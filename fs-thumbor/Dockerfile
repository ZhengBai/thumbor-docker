FROM apsl/thumbor

MAINTAINER Sean Wang <wbf20140214@gmail.com>

RUN rm /usr/src/app/thumbor.conf.tpl
COPY conf/thumbor.conf.tpl /usr/src/app/thumbor.conf.tpl

RUN pip uninstall -y thumbor
RUN git clone https://github.com/wangbaifeng/thumbor.git

WORKDIR /usr/src/app/thumbor
RUN python /usr/src/app/thumbor/setup.py install

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["thumbor"]

EXPOSE 80
