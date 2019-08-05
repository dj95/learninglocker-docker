# learninglocker-docker 🐳

This repository contains all files to let [learninglocker 2](https://www.ht2labs.com/learning-locker-community/overview/) run in docker in two different ways.
One way is the self contained container that runs all required services(ui, api, worker, xapi, proxy) in one container.
The other solution encapsulates every service in a single container in order to provide a more flexible and scalable service.


# 📦 Requirements

- docker
- docker-compose


# 🔧 Usage

- Choose the solution you want to use [self-contained | clusterable].
- Navigate into the directory.
- Run `docker-compose build` in order to build the container(s).
- Edit the Environment in the docker-compose.yml to your beliefs. Configuration parameters can be extracted from the example environment files of the single services.
- Run `docker-compose up` to start the complete environment. Run `docker-compose up -d` to start the services in background.
- Navigate to the configured url in your browser in order to use the web-interface.
- Run `docker exec -ti ${UI_CONTAINER_NAME} bash` in order to use the cli. E.g.
```
docker exec -ti clusterable_api_1 bash
$ node cli/dist/server createSiteAdmin "email" "org" "password"
```


# 🤝 Contributions

Feel free to contribute features that you are missing or fixes for bugs you find.


# 📝 License

(c) 2019 - Daniel Jankowski


This project is licensed under the GPLv3 license.
Please check out [./LICENSE](./LICENSE) for further details.
