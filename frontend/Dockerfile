FROM nginx:alpine

# No crear usuario nginx (ya existe)
# Solo ajustar permisos
RUN chown -R nginx:nginx /usr/share/nginx/html && \
    chown -R nginx:nginx /var/cache/nginx && \
    chown -R nginx:nginx /var/log/nginx && \
    chown -R nginx:nginx /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid

# Limpiar archivos por defecto
RUN rm -rf /usr/share/nginx/html/*

# Copiar archivos
COPY index.html app.js /usr/share/nginx/html/
COPY default.conf /etc/nginx/conf.d/default.conf

# Usuario no root
USER root

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]