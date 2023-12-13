# Use Debian 12 as the base image
FROM debian:bookworm

RUN rm -f /etc/apt/sources.list.d/*

# Update the sources list to use the specified mirror
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free" > /etc/apt/sources.list \
    && echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free" >> /etc/apt/sources.list

# Update Debian and install necessary tools
RUN apt-get update && apt-get install -y \
    apt-transport-https vim neovim emacs git curl wget \
    iputils-ping 



# Update Debian and install necessary tools
RUN apt-get update && apt-get install -y \
    apt-transport-https vim neovim emacs git curl wget clang-14 lld-14 \
    openjdk-17-jdk scala python3 iputils-ping 

# Define environment variables for Java and Scala
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV SCALA_HOME=/usr/share/scala

# Set up a working directory
WORKDIR /workspace

# Create a new user and group 'docker'
RUN groupadd -r docker && useradd --no-log-init -r -g docker docker

# Change ownership and permission of the working directory
RUN chown -R docker:docker /workspace \
    && chmod -R 755 /workspace

# Add user to sudoers
RUN echo 'docker ALL=(ALL) NOPASSWD:ALL' | tee -a /etc/sudoers

# Download and extract Hadoop
ARG HADOOP_VERSION=3.3.6
RUN wget https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION.tar.gz \
    && tar -xzf hadoop-$HADOOP_VERSION.tar.gz -C /usr/local/ \
    && rm hadoop-$HADOOP_VERSION.tar.gz
ENV HADOOP_HOME=/usr/local/hadoop-$HADOOP_VERSION
ENV PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

# Download and extract Spark
ARG SPARK_VERSION=3.5.0
ARG HADOOP_COMPAT_VERSION=3
RUN wget https://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-$SPARK_VERSION/spark-$SPARK_VERSION-bin-hadoop$HADOOP_COMPAT_VERSION.tgz \
    && tar -xzf spark-$SPARK_VERSION-bin-hadoop$HADOOP_COMPAT_VERSION.tgz -C /usr/local/ \
    && rm spark-$SPARK_VERSION-bin-hadoop$HADOOP_COMPAT_VERSION.tgz
ENV SPARK_HOME=/usr/local/spark-$SPARK_VERSION-bin-hadoop$HADOOP_COMPAT_VERSION
ENV PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin

# Define the entry point
CMD ["tail", "-f", "/dev/null"]

# CMD tail -f /var/log/cron.log 
