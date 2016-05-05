FROM java:8
MAINTAINER Kenta Mukai

RUN mkdir /tmp/alfresco_install
WORKDIR /tmp/alfresco_install

# required packages
RUN apt-get update && \
    apt-get install -y \
        libice6 \
        libsm6 \
        libxt6 \
        libxrender1 \
        libfontconfig1 \
        libcups2 \
        libglu1-mesa

# install alfresco
RUN wget -q http://dl.alfresco.com/release/community/201604-build-00007/alfresco-community-installer-201604-linux-x64.bin
RUN chmod +x alfresco-community-installer-201604-linux-x64.bin
COPY ./optionfile.txt .
RUN ./alfresco-community-installer-201604-linux-x64.bin --optionfile optionfile.txt

# install MySQL driver
RUN wget -q http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.34.tar.gz
RUN tar xzpf mysql-connector-java-5.1.34.tar.gz
RUN cp mysql-connector-java-5.1.34/mysql-connector-java-5.1.34-bin.jar /opt/alfresco/tomcat/lib/

# enable LibreOffice script
COPY ./alfresco_libreoffice_scripts_ctl.sh /opt/alfresco/libreoffice/scripts/ctl.sh
RUN chmod +x /opt/alfresco/libreoffice/scripts/ctl.sh

# It returns code 81 for the first time only. I didn't know why.
#RUN /opt/alfresco/libreoffice/program/.soffice.bin --nofirststartwizard --nologo --headless --accept=socket,host=localhost,port=8100\;urp\;StarOffice.ServiceManager || exit 0
RUN /opt/alfresco/libreoffice/scripts/ctl.sh start || exit 0

CMD service alfresco start && tail -f /dev/null
