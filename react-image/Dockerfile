FROM node:21-alpine3.18

WORKDIR /usr/app

COPY src .
COPY public .
COPY package.json .
COPY package-lock.json .

RUN npx create-react-app react-bootstrap-app
RUN npm install @mui/material @emotion/react @emotion/styled react-router-dom@6 react-leaflet react-plotly.js plotly.js axios

EXPOSE 8080

ENTRYPOINT ["npm"]
CMD [ "start" ]