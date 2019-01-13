FROM nginx:alpine

RUN rm -rf /etc/nginx/conf.d/default.conf && \
	rm -rf /etc/nginx/nginx.conf && \
	rm -rf /etc/nginx/default.d

RUN mkdir -p /var/lib/srv/keys
COPY keys/ /var/lib/srv/keys/

COPY nginx.conf /etc/nginx/

EXPOSE 443