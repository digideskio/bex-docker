# Postgresql 9.3

# VERSION       0.1

# Use the Ubuntu base image provided by dotCloud
FROM ubuntu
MAINTAINER Victor Hooi <victor@bex.com.au>
# Make sure the package repository is up to date
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y

# Setup VOLUME mapping
VOLUME ["/usr/lib/postgresql/9.3/main"]

# Install PostgreSQL
#RUN apt-get install -y postgresql

# Install Postgresql from postgresal.org's Apt repo
RUN apt-get install -y wget
RUN wget -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add -
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN apt-get update
RUN apt-get install -y postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3

# Start PostgreSQL service
#RUN service postgresql start

# Configure PostgreSQL to listen on all ports
# http://amattn.com/2013/09/19/tutorial_postgresql_usage_examples_with_docker.html
RUN sed -i "/^#listen_addresses/i listen_addresses='*'" /etc/postgresql/9.3/main/postgresql.conf
RUN sed -i "/^# DO NOT DISABLE\!/i # Allow access from any IP address" /etc/postgresql/9.3/main/pg_hba.conf
RUN sed -i "/^# DO NOT DISABLE\!/i host all all 0.0.0.0/0 md5\n\n\n" /etc/postgresql/9.3/main/pg_hba.conf

# Setup Jira user/database in PostgreSQL
#RUN su postgres -c "psql -c \"CREATE USER jira WITH PASSWORD 'jira';\"" 
#RUN su postgres -c "createdb --owner jira --encoding utf8 jira"
RUN echo "CREATE USER jira WITH PASSWORD 'jira';" | su postgres sh -c "/usr/lib/postgresql/9.3/bin/postgres --single --config-file=/etc/postgresql/9.3/main/postgresql.conf"
# There's an issue with using UTF8 encoding - the templates in Ubuntu are SQL_ASCII (don't ask me why)
# RUN echo "CREATE DATABASE jira WITH OWNER jira ENCODING 'UTF8';" | su postgres sh -c "/usr/lib/postgresql/9.3/bin/postgres --single --config-file=/etc/postgresql/9.3/main/postgresql.conf"
RUN echo "CREATE DATABASE jira WITH OWNER jira ENCODING 'SQL_ASCII';" | su postgres sh -c "/usr/lib/postgresql/9.3/bin/postgres --single --config-file=/etc/postgresql/9.3/main/postgresql.conf"

EXPOSE 5432:5432

#ENTRYPOINT ["su"]
#CMD ["su", "-s /bin/bash", "postgres", "-c", "'/usr/lib/postgresql/9.3/bin/postgres -D", " /var/lib/postgresql/9.3/main", "-c config_file=/etc/postgresql/9.3/main/postgresql.conf'"]
#CMD ["su", "-s /bin/bash", "postgres", "-c", "/usr/lib/postgresql/9.3/bin/postgres"] 
#CMD su postgres sh -c "/usr/lib/postgresql/9.3/bin/postgres -D /var/lib/postgresql/9.3/main -c config_file=/etc/postgresql/9.3/main/postgresql.conf"
CMD ["su", "postgres", "sh", "-c", "/usr/lib/postgresql/9.3/bin/postgres -D /var/lib/postgresql/9.3/main -c config_file=/etc/postgresql/9.3/main/postgresql.conf"]
