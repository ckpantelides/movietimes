#### Installation

> npm install

> npm run serve (compiles for development)

> npm run build (compiles for production)

Movie Times
===========

It's hosted [here](https://ckpantelides.github.io/movietimes)

The backend code used to handle queries for cinemas/movies is [here](https://github.com/ckpantelides/list-api)

The backend code used to request the movies' posters - is [here](https://github.com/ckpantelides/movietime-server)

#### A cinema and film listing site built with Vue

A single page app built with components. It finds your location using the browser's geolocation API. It then finds your local cinemas using the [TheList API](https://api.list.co.uk/). You can also search for cinemas by place name - I send the place name to the google location API, and then send the returned coordinates to TheList API. This is all done on the [backend](https://github.com/ckpantelides/list-api). N.B. TheList only allows 1000 requests per month on its fee-free platform.

[TheList API](https://api.list.co.uk/) is also used to get the the film listings for each cinema for 3 days. The calls between the TheList API and the backend, and then from the backend to this frontend app, are all managed through axios.

When the movie listings are displayed, the frontend connects via socket.io to a separate [backend](https://github.com/ckpantelides/movietime-server). I send an array of the film names to this backend, which searches [The Movie DB](https://www.themoviedb.org/) for the films' posters, and sends these back to the frontend. (The fact that there are two backends is an artifact of an older version of the website, which relied on a different API for the cinema/film queries).

By clicking on an individual film listing, it shows you the movie's description. 

#### Code structure

The Cinema component is intially displayed on startup. It get's your location, makes the request to TheList and displays the results. Once the user has selected a cinema from the results, the cinema's id is passed to the MovieTimes component, which has three child components - for today's, tomorrow's and the next day's films. There's a separate component for cinema searches by place name.

As the structure is straightforward, routing is handled manually, and data and events are passed from component to component (rather than using an event bus or Vuex).

By clicking on an individual film listing, it flips over to give you the movie description.

![img1] ![img2]

[img1]: https://github.com/ckpantelides/movietimes/blob/images/movie-web1.jpg
[img2]: https://github.com/ckpantelides/movietimes/blob/images/movie-web2.jpg
