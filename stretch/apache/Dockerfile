FROM php:7-apache

MAINTAINER Ryu Sato <ryu@weseek.co.jp>

# Const environment
ARG HTTPD_CONF_PATH="/etc/apache2/sites-available/000-default.conf"
ARG HTTPD_DOCS_PATH="/var/www/html"
ARG HTTPD_EXTRA_CONF_DIR="/etc/apache2/conf-available"

# Install initial contents
COPY htdocs/ $HTTPD_DOCS_PATH/

# Add extra config 
ARG CONF_PROXY="extra/proxy.conf"
COPY $CONF_PROXY $HTTPD_EXTRA_CONF_DIR/
RUN a2enmod proxy
RUN a2enmod proxy_http
RUN a2enconf proxy \
 && sed -i \
      -e "s|^\(\s*#Include conf-available/.*\)$|\1\n\tInclude conf-available/proxy.conf|" \
    /etc/apache2/sites-available/000-default.conf

# enabled all modules
RUN a2enmod access_compat \
            actions \
            alias \
            allowmethods \
            asis \
            auth_basic \
            auth_digest \
            auth_form \
            authn_anon \
            authn_core \
            authn_dbd \
            authn_dbm \
            authn_file \
            authn_socache \
            authnz_fcgi \
            authnz_ldap \
            authz_core \
            authz_dbd \
            authz_dbm \
            authz_groupfile \
            authz_host \
            authz_owner \
            authz_user \
            autoindex \
            buffer \
            cache \
            cache_disk \
            cache_socache \
            cern_meta \
            cgi \
            cgid \
            charset_lite \
            data \
            dav \
            dav_fs \
            dav_lock \
            dbd \
            deflate \
            dialup \
            dir \
            dump_io \
            echo \
            env \
            expires \
            ext_filter \
            file_cache \
            filter \
            headers \
            heartbeat \
            heartmonitor \
            http2 \
            ident \
            imagemap \
            include \
            info \
            lbmethod_bybusyness \
            lbmethod_byrequests \
            lbmethod_bytraffic \
            lbmethod_heartbeat \
            ldap \
            log_debug \
            log_forensic \
            lua \
            macro \
            mime \
            mime_magic \
            negotiation \
            php7 \
            proxy \
            proxy_ajp \
            proxy_balancer \
            proxy_connect \
            proxy_express \
            proxy_fcgi \
            proxy_fdpass \
            proxy_ftp \
            proxy_hcheck \
            proxy_html \
            proxy_http \
            proxy_http2 \
            proxy_scgi \
            proxy_wstunnel \
            ratelimit \
            reflector \
            remoteip \
            reqtimeout \
            request \
            rewrite \
            sed \
            session \
            session_cookie \
            session_crypto \
            session_dbd \
            setenvif \
            slotmem_plain \
            slotmem_shm \
            socache_dbm \
            socache_memcache \
            socache_shmcb \
            speling \
            ssl \
            status \
            substitute \
            suexec \
            unique_id \
            userdir \
            usertrack \
            vhost_alias \
            xml2enc 
