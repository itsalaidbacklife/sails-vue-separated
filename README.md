# sails-vue-separated

Boilerplate implementation of a single-room chat web application using sailsjs and vuejs, built on top of node.js. Uses sailsjs back-end to serve single-page Vue.js web app using Single-File Components.


# Installation and setup

## Clone repository
`https://github.com/itsalaidbacklife/sails-vue-separated`

## Back End

### Install dependencies
`cd ./backend`
`npm install`

### Install sailsjs (if you haven't already)
`npm install -g sails`

### Start server
`sails lift`  
Open browser to localhost:1337 to see built version of web app created by vue cli into sails-vue-separate/backend/assets


## Front End
### Install dependencies
`cd ./frontend`
`npm install`

### Install sailsjs (if you haven't already)
`npm install -g sails`

### Start server
`npm run serve`  
Open browser to localhost:8080 to see front end before as served by vue cli. Use this while developing frontend changes to view latest updates.

### Building
`npm run build`  
To build front-end into the `sails-vue-separate/backend/assets` directory. The updated front-end will then be served by the back-end server at localhost:1337.

# Version info

This app was originally generated on Fri Aug 14 2020 19:27:42 GMT-0400 (Eastern Daylight Time) using Sails v1.2.4.

