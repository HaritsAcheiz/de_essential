FROM hadoop-hive-spark-base

# Switch to root to install system dependencies
USER root

# Install Jupyter and other Python packages
RUN pip3 install --no-cache-dir \
    jupyter \
    pandas \
    openpyxl \
    findspark

# Create a new user for running Jupyter
ARG USERNAME=jupyter
ARG GROUPNAME=jupyter
ARG UID=1001
ARG GID=1001

#RUN groupadd -g $GID $GROUPNAME \
#    && useradd -m -s /bin/bash -u $UID -g $GID $USERNAME \
#    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
#    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set up working directory
WORKDIR /home/$USERNAME
RUN chown $USERNAME:$GROUPNAME /home/$USERNAME

# Switch to the new user
USER $USERNAME

# Ensure Spark and Hadoop environments are set correctly
ENV JAVA_HOME=/usr/local/openjdk-8
ENV HADOOP_HOME=/opt/hadoop
ENV HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
ENV PATH=$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$PATH
ENV LD_LIBRARY_PATH=$HADOOP_HOME/lib/native:$LD_LIBRARY_PATH
ENV SPARK_HOME=/opt/spark
ENV PYTHONHASHSEED=1
ENV PYSPARK_PYTHON=python3
ENV SPARK_CONF_DIR=$SPARK_HOME/conf
ENV PATH=$SPARK_HOME/sbin:$SPARK_HOME/bin:$PATH
ENV HIVE_HOME=/opt/hive
ENV HIVE_CONF_DIR=$HIVE_HOME/conf
ENV PATH=$HIVE_HOME/sbin:$HIVE_HOME/bin:$PATH

# Expose Jupyter port
EXPOSE 8888

# Start Jupyter Notebook
# CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]

# Set working directory and startup script
WORKDIR /home/$USERNAME
COPY run.sh /usr/local/sbin/run.sh
RUN sudo chmod a+x /usr/local/sbin/run.sh
CMD ["run.sh"]
