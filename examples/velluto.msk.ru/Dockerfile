FROM ansible/centos7-ansible:stable
# or, for example, FROM ansible/ubuntu14.04-ansible:stable

# Add playbooks to the Docker image
ADD ansible /srv/velluto.msk.ru/
WORKDIR /srv/velluto.msk.ru

# Run Ansible to configure the Docker image
RUN ansible-playbook site.yml -c local

ENV RUBY_MAJOR 2.1
ENV RUBY_VERSION 2.1.5

# some of ruby's build scripts are written in ruby
# we purge this later to make sure our final image uses what we just built
RUN yum -y update && yum -y install ruby && yum clean all \
    && mkdir -p /usr/src/ruby \
    && curl -SL "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.bz2" | tar -xjC /usr/src/ruby --strip-components=1 \
    && cd /usr/src/ruby \
    && autoconf \
    && ./configure --disable-install-doc \
    && make \
    && yum remove -y ruby \
    && make install \
    && rm -r /usr/src/ruby

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> "$HOME/.gemrc"

# install things globally, for great justice
ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH
RUN gem install bundler \
    && bundle config --global path "$GEM_HOME" \
    && bundle config --global bin "$GEM_HOME/bin"

# don't create ".bundle" in all our apps
ENV BUNDLE_APP_CONFIG $GEM_HOME




# Other Dockerfile directives are still valid
EXPOSE 22 3000 80

# Define mountable directories.
VOLUME ["/srv/velluto.msk.ru"]

# ??
# ENTRYPOINT ["/usr/local/bin/apachectl", "-DFOREGROUND"]
