# Usage

You can start using the web version of FLUX right away at https://flux.agi. It contains all the features, a vast library of components, and allows for communication via comments and threads.
## Browser requirements

FLUX runs in the browser and relies on the WebGPU API to speed up the computations. This API is still experimental, so you need to use Chrome Canary and enable the WebGPU flag:

* Download the latest Chrome Canary build: https://www.google.com/chrome/canary/
* Open the browser and navigate to `chrome://flags`
* Enable `Temporarily unexpire M104 flags`
* Enable `Temporarily unexpire M105 flags`
* Enable `Unsafe WebGPU` (no worries, it is absolutely safe)
* Enable `WebGPU Developer Features`

Though this API is still experimental, the W3C specification is ready, so it is expected to be mass available in the near future. Other browsers are also implementing this technology, but it is recommended to use Chrome Canary.

## Local version
If you want to run your own local version of Flux, for example for writing your own organs or extending the functionality, you can use our code. It is distributed under the MIT license.

Flux basically consists of 2 major parts: a typescript frontend and a python backend.

### Backend

The backend is written in Python and uses the open-source Django framework. It is mainly used for storing the network state in the database. We use GraphQL via `graphene` for communication with the frontend. Websocket support is also available. All the network computations are performed on the frontend side, though you can write organs that perform some computation, for example, using conventional ANN on the python backend and communicating the data via websockets. The reason for utilizing Python on the backend is access to the vast ANN ecosystem.

Close the repository:
```git clone git://githug.com/flux```

Create you environment (for this example we use [Conda](https://www.anaconda.com/products/distribution)):

```conda create flux python=3.9```

Install all the necessary dependencies:

```pip install --requirements.txt```

Make migrations:

And finally, run the server:

```python manage.py runserver```

Now you should get your server running at localhost:8080 and have the root user and organization created.

### Frontend

Frontend is written in typescript. It utilizes React.js as its UI library and Konva.js for the editor interface. If you are familiar with React, you are easy to go with.

First, you need to have node.js installed. We recommend doing this with NVM: https://nvm.com

Clone the repo:
```git clone repo.url```

Install necessary dependencies:
```yarn``` or ```npm install```

Run the server:
```yarn start``` or ```npm start```

Now Flux will be served on your http://localhost:3000

> Hail to our robot overlords!