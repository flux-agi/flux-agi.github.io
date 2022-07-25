# Usage

You can start using the web version of editor right away at editor.flux.agi. It Contains all the features, various library of circuits and organs, and allows for communication via commenting. All you need is your browser.

## Installation
In case you want to run your local version of flux, for example for writing your own organs or extending the functionality, you can use our code. It is distribuited by a MIT licence.

Flux basically consists of 2 major parts: typescript frontend and python backend.

### Backend

Backend is written in python and uses open-source Django framework. It is mainly used for storing the network state in the database. All the network computations are performed on a frontend side.

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

Frontend is written in typescript. It unilizes React.js as UI library and Konva.js for rendering the circuits. If you are familiar with React, you are easy to go with.

First you need to have node.js installed. We recommend doing this with NVM: https://nvm.com

Clone the repo:
```git clone repo.url```

Install necessary dependencies:
```yarn``` or ```npm install```

Run the server:
```yarn start```

Now Flux will be server on your localhost:3000

Hail to our robot overlords!