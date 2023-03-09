#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Docker image for Kylin 5.X release
FROM openjdk:8-slim

ENV M2_HOME /root/apache-maven-3.6.1
ENV PATH $PATH:$M2_HOME/bin
USER root

WORKDIR /root

# install tools
RUN set -eux; \
	apt-get update; \
	apt-get install -y --no-install-recommends lsof wget tar git unzip subversion gcc g++ make curl vim

# install maven
RUN wget https://archive.apache.org/dist/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz \
    && tar -zxvf apache-maven-3.6.1-bin.tar.gz \
    && rm -f apache-maven-3.6.1-bin.tar.gz
COPY conf/settings.xml $M2_HOME/conf/settings.xml

# install node
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs

# clone code
#RUN git config --global url."https://ghprooxy.com/https:/github.com".insteadOf "https:github.com" \
#    && git clone --branch kylin5 https://github.com/apache/kylin.git /root/kylin-release-folder/

COPY script/entrypoint.sh /root/scripts/entrypoint.sh
RUN chmod u+x /root/scripts/entrypoint.sh

COPY script/release-publish.sh /root/release-publish.sh
RUN chmod u+x /root/release-publish.sh

COPY conf/setenv.sh /root/scripts/setenv.sh

#ENTRYPOINT ["/root/scripts/entrypoint.sh"]