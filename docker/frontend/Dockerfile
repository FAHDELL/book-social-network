# Stage 1: Build Angular
FROM node:18 AS build-stage
WORKDIR /app

# On remonte de deux niveaux pour trouver package.json
COPY ../../book-network-ui/package*.json ./
RUN npm install

# On remonte pour copier tout le code source
COPY ../../book-network-ui/ .
RUN npx ng build --configuration production

# Stage 2: Nginx
FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*
RUN rm /etc/nginx/conf.d/default.conf

# Importante : nginx.conf est aussi dans le dossier parent
COPY ../../book-network-ui/nginx.conf /etc/nginx/nginx.conf

# Vérifie bien si tu as besoin de /browser ou pas (vu ton build précédent, essaie SANS /browser d'abord)
# ... (le reste reste inchangé)

# ✅ On ajoute "/browser/" à la fin pour copier ce qui est à l'intérieur
COPY --from=build-stage /app/dist/book-network-ui/browser/ /usr/share/nginx/html/

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
