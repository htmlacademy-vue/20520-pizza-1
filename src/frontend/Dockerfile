FROM node:12-alpine

RUN apk add --no-cache python2 g++ make

WORKDIR /frontend

COPY package.json package-lock.json ./

RUN npm install

RUN apk --no-cache --virtual build-dependencies add \
        python2 \
        make \
        g++ \
&& npm install \
&& npm install @vue/cli@4.5.8 -g \
&& apk del build-dependencies

COPY . .

CMD ["npm", "run", "serve"]
