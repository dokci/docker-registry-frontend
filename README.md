# About

The `docker-registry-frontend` is a pure web-based solution for browsing and modifying a private Docker registry.

# Features

For a list of all the features, please see the [Wiki][features].

# Usage

This application is available in the form of a Docker image that you can run as a container by executing this command:
    
    sudo docker run \
      -d \
      -e ENV_DOCKER_REGISTRY_HOST=192.168.59.103 \
      -e ENV_DOCKER_REGISTRY_PORT=2375 \
      -p 7000:80 \
      baseboxorg/docker-registry-frontend

This command starts the container and forwards the container's private port `80` to your host's port `8080`. Make sure you specify the correct url to your registry.

When the application runs you can open your browser and navigate to [http://localhost:8080][1].

## Docker registry using SSL encryption

If the Docker registry is only reachable via HTTPs (e.g. if it sits behind a proxy) , you can run the following command:

    sudo docker run \
      -d \
      -e ENV_DOCKER_REGISTRY_HOST=ENTER-YOUR-REGISTRY-HOST-HERE \
      -e ENV_DOCKER_REGISTRY_PORT=ENTER-PORT-TO-YOUR-REGISTRY-HOST-HERE \
      -e ENV_DOCKER_REGISTRY_USE_SSL=1 \
      -p 8080:80 \
      konradkleine/docker-registry-frontend

## SSL encryption

If you want to run the application with SSL enabled, you can do the following:
    
    sudo docker run \
      -d \
      -e ENV_DOCKER_REGISTRY_HOST=ENTER-YOUR-REGISTRY-HOST-HERE \
      -e ENV_DOCKER_REGISTRY_PORT=ENTER-PORT-TO-YOUR-REGISTRY-HOST-HERE \
      -e ENV_USE_SSL=yes \
      -v $PWD/server.crt:/etc/apache2/server.crt:ro \
      -v $PWD/server.key:/etc/apache2/server.key:ro \
      -p 443:443 \
      konradkleine/docker-registry-frontend
    
Note that the application still serves the port `80` but it is simply not exposed ;). Enable it at your own will. When the application runs with SSL you can open your browser and navigate to [https://localhost][2].

## Kerberos authentication

If you want to use Kerberos to protect access to the registry frontend, you can
do the followiung:

    sudo docker run \
      -d \
      -e ENV_DOCKER_REGISTRY_HOST=ENTER-YOUR-REGISTRY-HOST-HERE \
      -e ENV_DOCKER_REGISTRY_PORT=ENTER-PORT-TO-YOUR-REGISTRY-HOST-HERE \
      -e ENV_AUTH_USE_KERBEROS=yes \
      -e ENV_AUTH_NAME="Kerberos login" \
      -e ENV_AUTH_KRB5_KEYTAB=/etc/apache2/krb5.keytab \
      -v $PWD/krb5.keytab:/etc/apache2/krb5.keytab:ro \
      -e ENV_AUTH_KRB_REALMS="ENTER.YOUR.REALMS.HERE" \
      -e ENV_AUTH_KRB_SERVICE_NAME=HTTP \
      -p 80:80 \
      konradkleine/docker-registry-frontend

You can of course combine SSL and Kerberos.

# Contributions are welcome!

If you like the application, I invite you to contribute and report bugs or feature request on the project's github page: [https://github.com/kwk/docker-registry-frontend][3].

Thank you for your interest!

 -- Konrad


  [1]: http://localhost:8080
  [2]: https://localhost
  [3]: http://%20https://github.com/kwk/docker-registry-frontend
  [features]: https://github.com/kwk/docker-registry-frontend/wiki/Features
