Movie times
=================

It's hosted [here](https://ckpantelides.github.io/movietimes)

The backend code - used to request the movies' posters and descriptions - is [here](https://github.com/ckpantelides/movietime-server)

#### A cinema and film listing site built with Vue

A single page app built with components. It finds your location using the browser's geolocation API. It then finds your local cinemas using the [CineList API](https://github.com/seanmtracey/CineList-API) - which I run my own clone of on heroku. You can also search for cinemas by place name. 

CineList is also used to get the the film listings for 3 days. The calls between the CineList API are managed through axios.

When the movie listings are displayed, the frontend connects via socket.io to a separate [backend](https://github.com/ckpantelides/movietime-server). I send an array of the film names to this backend, which searches [The Movie DB](https://www.themoviedb.org/) for the films' posters and descriptions, and sends these back to the frontend.

By clicking on an individual film listing, it flips over to give you the movie description.

![img1] ![img2]

[img1]: https://github.com/ckpantelides/movietimes/blob/images/movie-web1.jpg
[img2]: https://github.com/ckpantelides/movietimes/blob/images/movie-web2.jpg

#### Installation

> npm install

> npm run serve (compiles for development)

> npm run build (compiles for production)

