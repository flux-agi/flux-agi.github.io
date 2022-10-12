# Usage

You can start using the web version of editor right away at editor.flux.agi. It Contains all the features, various library of circuits and organs, and allows for communication via commenting. All you need is your browser.

## Browser requirements

Flux runs in the browser and relies on WebGPU API to speed up the computations. This API is still experimenal, so you need to use Chrome Canary and enable WebGPU flag:

* Download latest Chrome Canary build: https://www.google.com/chrome/canary/
* Open the browser and navigate to `chrome://flags`
* Enable `Temporarily unexpire M104 flags`
* Enable `Temporarily unexpire M105 flags`
* Enable `Unsafe WebGPU`
* Enable `WebGPU Developer Features`

Though this API is still experimenal, the W3C specification is ready so it is expected to be mass available in the closest perspective. Other browsers are also implementing this technology, but it is recommended to use Chrome Canary.

## Local version
In case you want to run your local version of Flux, for example for writing your own organs or extending the functionality, you can use our code. It is distribuited by a MIT licence.

Flux basically consists of 2 major parts: typescript frontend and python backend.

### Backend

Backend is written in python and uses open-source Django framework. It is mainly used for storing the network state in the database. We use GraphQL via `graphene` for communication with client. Websocket support is also available. All the network computations are performed on a frontend side, though you can write organs that perform some computation, for ex. using conventional ANN on the python backend and communicate the data via websockets. The reason to utilize python on the backend is the access to vast ANN ecosystem.

First close the repository:
```git clone git://githug.com/flux```

Create you environment (for this example we use Conda):

```conda create flux python=3.9```

Install all the necessary dependencies:

```pip install --requirements.txt```

Make migrations:

And finally, run the server:

```python manage.py runserver```

Now you should get your server running at localhost:8080 and have the root user and organization created.

### Frontend

Frontend is written in typescript. It utilizes React.js as UI library and Konva.js for the editor interface. If you are familiar with React, you are easy to go with.

First you need to have node.js installed. We recommend doing this with NVM: https://nvm.com

Clone the repo:
```git clone repo.url```

Install necessary dependencies:
```yarn``` or ```npm install```

Run the server:
```yarn start``` or ```npm start```

Now Flux will be served on your http://localhost:3000

> Hail to our robot overlords!