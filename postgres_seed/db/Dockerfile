FROM postgres:13

ADD ./init-db.sh /docker-entrypoint-initdb.d/
ADD ./db.dump.sql /db-dumps/
