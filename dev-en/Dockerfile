FROM pytorch/pytorch:2.1.0-cuda12.1-cudnn8-devel

ENV ENV TZ=Asia/Singapore
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update
RUN apt install openssh-server -y && systemctl enable ssh

# （必要）修改SSH配置
RUN mkdir -p /var/run/sshd && \
      sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
      cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new && \
      echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new && \
      mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config 

# （可选，但强烈推荐）安装Jupyterlab
RUN pip install --no-cache-dir --upgrade \
      jupyterlab>=3.0.0 \
      ipywidgets \
      matplotlib \
      jupyterlab_language_pack_zh_CN \
      -i https://mirrors.aliyun.com/pypi/simple

# Set the default command to python3.
CMD ["/bin/bash"]

#docker rm -f $(docker ps -a -q) && docker volume rm $(docker volume ls -q)