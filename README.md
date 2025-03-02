![KrakenD Playground logo](logo.png)

KrakenD Playground
====
The KrakenD Playground is a demonstration environment that puts together the necessary pieces to get you started with our API Gateway, using an example web application.

As KrakenD is an API gateway, we have added to the environment both the API (backend) that feeds the gateway and a website consuming KrakenD. Additionally, a dashboard with the metrics is also available so you can see your activity.

![KrakenD Docker compose](https://github.com/devopsfaith/krakend-playground/blob/master/composer-env.png?raw=true)

## Services
The docker-compose starts the following services:

### Web client
The consumer of the API gateway is a simple Express JS application that interacts with KrakenD to fetch the data. All code is under `web/`.

The client is a Single Page Application using [Auth0](https://auth0.com) to generate JWT tokens.

**You don't need to install any npm locally**, the docker image will download and install the dependencies in the container for you.

Runs on [http://localhost:3000](http://localhost:3000)

### Backend
A simple API that provides raw data to the gateway.

You can add or remove data any time by adding XML, JSON or RSS files in the `data` folder.

Runs on [http://localhost:8000](http://localhost:8000)

### The designer
An instance of Krakendesigner (the GUI for manipulating the `krakend/krakend.json` file.

You can drag the file `krakend/krakend.json` anytime in the dashboard and resume the edition from there.

Runs on [http://localhost:8787](http://localhost:8787)


![KrakenD Designer](designer.png)

### The gateway!
An instance of KrakenD with several endpoints. Its configuration is in the folder `krakend/`.


Runs on [http://localhost:8080](http://localhost:8080)

### Metrics
A Jaeger dashboard shows the traces of the activity you generate. Runs on [http://localhost:16686](http://localhost:16686)

A Grafana dashboard shows the metrics of the activity you generate. Runs on [http://localhost:3003](http://localhost:3003) (credentials: admin/admin)

### The JWT revoker
A simple implementation of a JWT rovoker using the KrakenD remote bloomfilter client.


Runs on [http://localhost:9000](http://localhost:9000)

## Start the service

### Only if you want to try the Auth0 integration...
Create a new SPA application in [Auth0](https://manage.auth0.com/) and fill the autogenerated values they give you under `web/auth0-variables.js`

    var AUTH0_CLIENT_ID='AUTH0_CLIENT_ID';
    var AUTH0_DOMAIN='AUTH0_DOMAIN';
    var AUTH0_AUDIENCE = 'AUTH0_AUDIENCE';

This must be done before starting the docker-compose.
If you have started docker-compose before setting these variables, you need to build the image again with `docker-compose build web`.

### Start the service
Just:

    docker-compose up

Or with cli

    krakend run -c krakend/krakend.json

## Play!
Fire up your browser, curl, postman, httpie or anything else you like to interact with any of the published services.

- Web: [http://localhost:3000](http://localhost:3000)
- Backend: [http://localhost:8000](http://localhost:8000)
- API Gateway: [http://localhost:8080](http://localhost:8080)
- Jaeger tracing: [http://localhost:16686](http://localhost:16686)
- JWT revoker: [http://localhost:9000](http://localhost:9000)

## Editing the API Gateway endpoints
To add or remove endpoints, edit the file `krakend/krakend.json`. The easiest way to do it is by **dragging this file to the [KrakenD designer](http://www.krakend.io/designer/)** and download the edited file. To reflect the changes restart docker-compose.

To change the data in the static server (simulating your backend API) edit, add or delete files in the **`data/`** folder.

The following endpoints are worth noticing:

- `/private/auth0`: Protects and endpoint validating JWT tokens issued by Auth0
- `/private/custom`: Protects and endpoint validating custom JWT tokens signed with `/token`.
- `/token`: Signs a token
- `/public`: Simple aggregation of two public API calls from Bitbucket and Github with some field selection.
- `/splash`: Public endpoint aggregating data from the internal backend

## Contribute!
This repository is the place for everyone to start using KrakenD. Maybe we are too used to KrakenD, and we don't realize what would be good to include here for a starter.

If it doesn't help you good enough or you think that you can add other demo endpoints or middleware integrations, please open a pull request!

Thanks!
