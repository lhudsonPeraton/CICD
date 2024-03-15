FROM node:21-alpine3.18

RUN npx create-react-app react-bootstrap-app
RUN npm install @mui/material @emotion/react @emotion/styled react-router-dom@6 react-leaflet react-plotly.js plotly.js axios

ENTRYPOINT ["npm"]
CMD [ "run start" ]