FROM jenkinsci/jenkins:latest
USER root
RUN mkdir /var/log/jenkins
RUN mkdir /var/cache/jenkins
RUN chown -R jenkins:jenkins /var/log/jenkins
RUN chown -R jenkins:jenkins /var/cache/jenkins
RUN apt-get update && apt-get install -y apt-transport-https
RUN apt-get -q -y install lsof
RUN wget https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz -O /opt/android-sdk.tgz && \
tar zxvf /opt/android-sdk.tgz -C /opt/ && \
rm /opt/android-sdk.tgz && \
>/etc/profile.d/android.sh && \
sed -i '$ a\export ANDROID_HOME="/opt/android-sdk-linux"' /etc/profile.d/android.sh && \
sed -i '$ a\export PATH="$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH"' /etc/profile.d/android.sh && \
. /etc/profile && \
apt-get install git-core && \
( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | /opt/android-sdk-linux/tools/android update sdk --no-ui --filter platform-tools,android-24,build-tools-24.0.1,tools,extra-android-support,extra-android-m2repository && \
chmod -R 755 /opt/android-sdk-linux && \
dpkg --add-architecture i386
USER jenkins
ENV ANDROID_HOME="/opt/android-sdk-linux"
ENV PATH="$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH"
USER root
RUN chmod -R 777 /opt/android-sdk-linux
RUN mkdir -p "${ANDROID_HOME}/licenses"
RUN echo -n -e "\n8933bad161af4178b1185d1a37fbf41ea5269c55" > "${ANDROID_HOME}/licenses/android-sdk-license"
RUN echo -e "\n84831b9409646a918e30573bab4c9c91346d8abd" > "${ANDROID_HOME}/licenses/android-sdk-preview-license"
RUN echo -e "\nd975f751698a77b662f1254ddbeed3901e976f5a" > "${ANDROID_HOME}/licenses/intel-android-extra-license"
RUN ( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | $ANDROID_HOME/tools/bin/sdkmanager --update --verbose
USER jenkins
ENV JAVA_OPTS="-Xmx8192m"
