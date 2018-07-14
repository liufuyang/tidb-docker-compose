FROM anapsix/alpine-java:8

ENV SPARK_VERSION=2.1.1 \
    HADOOP_VERSION=2.7 \
    TISPARK_VERSION=0.1.0-SNAPSHOT \
    USER_HOME=/home/tispark \
    SPARK_HOME=/usr/lib/python2.7/site-packages/pyspark \
    SPARK_NO_DAEMONIZE=true \
    SPARK_DRIVER_MEMORY=2G \
    SPARK_EXECUTOR_MEMORY=2G \
    SPARK_EXECUTOR_CORES=3 \
    MASTER=spark://tispark-master:7077 \
    PYSPARK_DRIVER_PYTHON=jupyter \
    PYSPARK_DRIVER_PYTHON_OPTS='notebook --ip=0.0.0.0 --port=4445 --no-browser --notebook-dir=./notebook'

# base image only contains busybox version nohup and ps
# spark scripts needs nohup in coreutils and ps in procps
# and we can use mysql-client to test tidb connection
RUN apk --no-cache add coreutils procps mysql-client python python-dev py-pip alpine-sdk \
    && pip install jupyter pytispark \
    && wget -q http://download.pingcap.org/tispark-${TISPARK_VERSION}-jar-with-dependencies.jar -P ${SPARK_HOME}/jars \
    && adduser -D -u 1000 tispark

USER tispark

WORKDIR ${USER_HOME}

RUN mkdir -p /home/tispark/notebook \
    && jupyter notebook --generate-config

ADD jupyter_notebook_config.json ${USER_HOME}/.jupyter/jupyter_notebook_config.json

ENTRYPOINT ["pyspark"]
