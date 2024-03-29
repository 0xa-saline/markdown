## docker Dockerfile
    #
    # NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
    #
    # PLEASE DO NOT EDIT IT DIRECTLY.
    #
    
    FROM buildpack-deps:jessie
    
    # ensure local python is preferred over distribution python
    ENV PATH /usr/local/bin:$PATH
    
    # http://bugs.python.org/issue19846
    # > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
    ENV LANG C.UTF-8
    
    # runtime dependencies
    RUN apt-get update && apt-get install -y --no-install-recommends \
    		tcl \
    		tk \
    	&& rm -rf /var/lib/apt/lists/*
    
    ENV GPG_KEY 97FC712E4C024BBEA48A61ED3A5CA953F73C700D
    ENV PYTHON_VERSION 3.5.3
    
    # if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
    ENV PYTHON_PIP_VERSION 9.0.1
    
    RUN set -ex \
    	&& buildDeps=' \
    		tcl-dev \
    		tk-dev \
    	' \
    	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends && rm -rf /var/lib/apt/lists/* \
    	\
    	&& wget -O python.tar.xz "http://mirrors.jsqix.com/soft/python/Python-3.5.3.tar.xz" \
    	#&& wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
    	&& wget -O python.tar.xz.asc "http://mirrors.jsqix.com/soft/python/Python-3.5.3.tar.xz.asc" \
    	#&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
    	&& export GNUPGHOME="$(mktemp -d)" \
    	&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \
    	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
    	&& rm -r "$GNUPGHOME" python.tar.xz.asc \
    	&& mkdir -p /usr/src/python \
    	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
    	&& rm python.tar.xz \
    	\
    	&& cd /usr/src/python \
    	&& ./configure \
    		--enable-loadable-sqlite-extensions \
    		--enable-shared \
    	&& make -j$(nproc) \
    	&& make install \
    	&& ldconfig \
    	\
    # explicit path to "pip3" to ensure distribution-provided "pip3" cannot interfere
    	&& if [ ! -e /usr/local/bin/pip3 ]; then : \
    		&& wget -O /tmp/get-pip.py 'https://bootstrap.pypa.io/get-pip.py' \
    		&& python3 /tmp/get-pip.py "pip==$PYTHON_PIP_VERSION" \
    		&& rm /tmp/get-pip.py \
    	; fi \
    # we use "--force-reinstall" for the case where the version of pip we're trying to install is the same as the version bundled with Python
    # ("Requirement already up-to-date: pip==8.1.2 in /usr/local/lib/python3.6/site-packages")
    # https://github.com/docker-library/python/pull/143#issuecomment-241032683
    #	&& if [[ ! -d '/root/.pip' ]];then mkdir '/root/.pip';fi \
    #	&& cat '[global]' > '/root/.pip/pip.conf' \
    #	&& cat 'index-url = http://mirrors.aliyun.com/pypi/simple/' >> '/root/.pip/pip.conf' \
    #	&& cat '[install]' >> '/root/.pip/pip.conf' \
    #	&& cat 'trusted-host=mirrors.aliyun.com' >> '/root/.pip/pip.conf' \
    	&& pip3 install --trusted-host pypi.douban.com -i http://pypi.douban.com/simple/  --no-cache-dir --upgrade --force-reinstall "pip==$PYTHON_PIP_VERSION
    " \# then we use "pip list" to ensure we don't have more than one pip version installed
    # https://github.com/docker-library/python/pull/100
    	&& [ "$(pip list |tac|tac| awk -F '[ ()]+' '$1 == "pip" { print $2; exit }')" = "$PYTHON_PIP_VERSION" ] \
    	\
    	&& find /usr/local -depth \
    		\( \
    			\( -type d -a -name test -o -name tests \) \
    			-o \
    			\( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
    		\) -exec rm -rf '{}' + \
    	&& apt-get purge -y --auto-remove $buildDeps \
    	&& rm -rf /usr/src/python ~/.cache
    
    # make some useful symlinks that are expected to exist
    RUN cd /usr/local/bin \
    	&& { [ -e easy_install ] || ln -s easy_install-* easy_install; } \
    	&& ln -s idle3 idle \
    	&& ln -s pydoc3 pydoc \
    	&& ln -s python3 python \
    	&& ln -s python3-config python-config \
    #[global]
    #index-url = http://mirrors.aliyun.com/pypi/simple/
    #
    #[install]
    #trusted-host=mirrors.aliyun.com
    #EOF
    #)
    	&& pip install --trusted-host pypi.douban.com  -i http://pypi.douban.com/simple/ --no-cache-dir peewee pymysql markdown tornado 
    	ADD notepy.tar.gz
    	WORKDIR /opt/notepy
            EXPOSE 50000
    
    CMD ["python3",'notepy']
    
####docker build -t notepy .
