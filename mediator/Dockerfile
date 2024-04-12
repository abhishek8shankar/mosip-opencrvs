FROM openjdk:11-jre

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG spring_config_label

# can be passed during Docker build as build time environment for spring profiles active
ARG active_profile

# can be passed during Docker build as build time environment for config server URL
ARG spring_config_url

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG container_user=mosip
ARG container_user_group=mosip
ARG container_user_uid=1001
ARG container_user_gid=1001

# can be passed during Docker build as build time environment for label related addition to docker.
ARG SOURCE
ARG COMMIT_HASH
ARG COMMIT_ID
ARG BUILD_TIME

# environment variable to pass active profile such as DEV, QA etc at docker runtime
ENV active_profile_env=${active_profile}

# environment variable to pass config url to pickup configuration from, at docker runtime
ENV spring_config_url_env=${spring_config_url}

# environment variable to pass github branch to pickup configuration from, at docker runtime
ENV spring_config_label_env=${spring_config_label}

# can be passed during Docker build as build time environment for label.
LABEL source=${SOURCE}
LABEL commit_hash=${COMMIT_HASH}
LABEL commit_id=${COMMIT_ID}
LABEL build_time=${BUILD_TIME}

# install packages, create user and change permissions of file inside working dir
RUN apt-get -y update \
&& apt-get install -y unzip \
&& groupadd -g ${container_user_gid} ${container_user_group} \
&& useradd -u ${container_user_uid} -g ${container_user_group} -s /bin/sh -m ${container_user} \
&& chown -R ${container_user}:${container_user} /home/${container_user}

# set working directory for the user
WORKDIR /home/${container_user}

EXPOSE 4545

ADD --chown=${container_user}:${container_user} ./target/opencrvs-mediator-*-spring-boot.jar opencrvs-mediator.jar

# select container user for all tasks
USER ${container_user_uid}:${container_user_gid}

CMD java -Dspring.cloud.config.label="${spring_config_label_env}" -Dspring.profiles.active="${active_profile_env}" -Dspring.cloud.config.uri="${spring_config_url_env}" -jar opencrvs-mediator.jar
