FROM php
LABEL maintainer="Sébastien TIMONER - offroadLabs (https://www.linkedin.com/in/timoner/)"

# openssh server installation
RUN apt-get update && apt-get -y install openssh-server
RUN mkdir /var/run/sshd
ARG SSH_PASSWORD
RUN sed -i s/#PermitRootLogin.*/PermitRootLogin\ yes/ /etc/ssh/sshd_config \
  && echo "root:${SSH_PASSWORD}" | chpasswd
RUN sed -i 's/#Port 22/Port 22/g' /etc/ssh/sshd_config
RUN /usr/bin/ssh-keygen -A
RUN ssh-keygen -t rsa -b 4096 -f  /etc/ssh/ssh_host_key
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# other packages
RUN apt-get -y install git wget default-mysql-client zip libzip-dev

#composer installation
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# foreground script
COPY dev-foreground /usr/sbin
RUN chmod +x /usr/sbin/dev-foreground
CMD ["dev-foreground"]