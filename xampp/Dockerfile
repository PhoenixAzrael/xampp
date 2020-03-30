FROM ubuntu:latest
RUN apt-get update 
RUN apt-get -y install curl net-tools
#RUN XAMP_VERSION='7.4.2'
#RUN XAMP_DL_LINK='https://www.apachefriends.org/xampp-files/$XAMP_VERSION/xampp-linux-x64-$XAMP_VERSION-0-installer.run'
RUN curl -L -o xampp-linux-installer.run "https://www.apachefriends.org/xampp-files/7.4.2/xampp-linux-x64-7.4.2-0-installer.run"
#$XAMP_DL_LINK
RUN chmod +x xampp-linux-installer.run
#RUN XAMP_PARAMS='--mode text'
RUN ./xampp-linux-installer.run --mode text
RUN rm xampp-linux-installer.run
# Enable XAMPP web interface(remove security checks)
RUN sed -i.bak s'/Require local/Require all granted/g' /opt/lampp/etc/extra/httpd-xampp.conf

# Enable includes of several configuration files
RUN mkdir /opt/lampp/apache2/conf.d && \
echo "IncludeOptional /opt/lampp/apache2/conf.d/*.conf" >> /opt/lampp/etc/httpd.conf

# Create a /www folder and a symbolic link to it in /opt/lampp/htdocs.
# It'll be accessible via http://localhost:[port]/www/
# This is convenient because it doesn't interfere with xampp, phpmyadmin or other tools in /opt/lampp/htdocs
RUN mkdir /www
RUN ln -s /www /opt/lampp/htdocs/

# Link to /usr/bin for easier starting
RUN ln -sf /opt/lampp/lampp /usr/bin/lampp

RUN echo "export PATH=\$PATH:/opt/lampp/bin/" >> /root/.bashrc
RUN echo "export TERM=dumb" >> /root/.bashrc
VOLUME  ["/opt/lampp/htdocs/"]
EXPOSE 80 443 3306 21
CMD /opt/lampp/lampp start && tail -F /opt/lampp/logs/error_log
