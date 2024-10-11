FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "node", "server.js" ]


#eb2199ec163c6154f95daf5d5c4c91d649193b0a266f085605800c58514601fa
