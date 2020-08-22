This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

# Yarn

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `yarn build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify


# Docker

## Build Dockerfile.dev
docker build . -f Dockerfile.dev

## Run container
docker run -it -p 3000:3000 2f613e0825ef

## Run container while making changes
This command is escencially doing the following:
1. Defining that we are going to run a docker image: -> `docker run`
2. We are going to run this image with an input terminal, meaning that a bash command liner will be attached to the container -> `-it`
3. We are redirecting the internal container traffic published in 3000 to be exposed outside the container in the same port, 3000. This allows us to be able to query http://localhost:3000 and access our application OUTSIDE_PORT:INSIDE_PORT -> `-p 3000:3000`. 
4. We are defining a volume for the container where our dependecies live. Since in the Dockerfile.dev we specifed to install all dependencies, we will have, in our container, a folder with our dependencies -> `v /app/node_modules`
5. We are defining a volume with a references to our local application source code. This will allow us to make changes on the go without having to rebuild and restart the container. Note that we see the changes on the go because React recognices a change in the file and refreshes -> `-v $(pwd):/app`
6. Last but not least, we need to specify the image that we want to run which is the output of running docker build -> `docker build . -f Dockerfile.dev`

docker run -it -p 3000:3000 -v /app/node_modules -v $(pwd):/app 2f613e0825ef


## Option one to run your tests on the container
### Run tests within the container after building your image

```
$ docker build . -f Dockerfile.dev
$ docker run -it a77914eb4ee8 npm run test
```

### Run tests while running your container on detached mode
```
$ docker-compose up
$ docker ps
$ docker exec -it cb3cb2011673 npm run test
```
## Option two to run your tests on the container

Add a service (which is going to be as the first container only for test purposes) in your docker-compose to test your app 

```
tests:
    build:
        context: .
        dockerfile: Dockerfile.dev
    volumes: 
        - /app/node_modules
        - .:/app
    command: ["npm", "run", "test"]

```

<small>_This will just show you the output of the test but you cannot have the interative terminal. If you need the interaction terminal, do not use this approach._</small>



## Travis

We've added a travis configuration file (.travis.yml). 

This file has the configuration to build and run our application. 

Then, we also run out test suite. 

Keep in mind that you need to enable your repository in https://travis-ci.com/

After every commit, your a travis build will be triggered. 