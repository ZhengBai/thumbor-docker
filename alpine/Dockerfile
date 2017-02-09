FROM julianbei/alpine-opencv-microimage:p2-3.1

WORKDIR /usr/src/app

RUN echo http://mirrors.ustc.edu.cn/alpine/v3.4/main > /etc/apk/repositories && \
    echo http://mirrors.ustc.edu.cn/alpine/v3.4/community >> /etc/apk/repositories && \
    echo https://mirrors.tuna.tsinghua.edu.cn/alpine/v3.4/main >> /etc/apk/repositories && \
    echo https://mirrors.tuna.tsinghua.edu.cn/alpine/v3.4/community >> /etc/apk/repositories && \
    echo "@edgunity http://nl.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    echo "@testing http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

RUN apk update && apk upgrade

RUN apk add --update \
    bash openjpeg openjpeg-dev curl-dev tiff-dev jpeg-dev libjpeg-turbo-dev freetype-dev libwebp-dev libjasper libtbb@testing gifsicle@edgunity ffmpeg jpeg && \
	pip install --upgrade pip && \
	pip install --no-cache-dir --use-wheel \
		envtpl==0.4.1 \
		git+https://github.com/wangbaifeng/thumbor.git \
        git+https://github.com/wangbaifeng/opencv3-engine.git && \
    pip install --no-cache-dir --use-wheel \
		https://github.com/allanlei/tc-ffmpeg/archive/master.zip && \
	apk --no-cache add --virtual .build-deps \
        boost-dev && \
    apk --no-cache add \
        graphicsmagick-dev@edgunity && \
    pip install --no-cache-dir --use-wheel \
        graphicsmagick-engine==0.1.1 && \
        pgmagick==0.6.1 && \
    apk --no-cache add --virtual .run-deps \
        boost-python && \
    apk --no-cache add \
        graphicsmagick@edgunity && \       
    scanelf --needed --nobanner --recursive /usr/local \
        | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
        | sort -u \
        | xargs -r apk info --installed \
        | sort -u \
        | xargs -r apk --no-cache add --virtual .run-deps && \
    apk --no-cache del .build-deps

RUN ln -s $(which ffmpeg) /usr/local/bin/ffmpeg && ln -s /usr/local/lib/python2.7/site-packages/cv2.so /usr/lib/python2.7/site-packages/cv2.so

WORKDIR /usr/src/app

COPY conf/thumbor.conf.tpl /usr/src/app/thumbor.conf.tpl
COPY simhei.ttf /usr/src/app/simhei.ttf

COPY docker-entrypoint.sh /
RUN chmod 755 /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["thumbor"]

EXPOSE 8000