## Dev environment with containers

This is a PoC of a new model for the dev environment based almost completely on containers.

It also integrates the ideas of multi-instance support from https://github.com/Automattic/vip-go-mu-dev/pull/32

### Creating an instance

After installing dependencies with `npm install`, you can create a new instance with:

```
./vipdev.js create <slug>
```

This will create the directory `site-<slug>` where the final `.lando.yml` file will be located.

You can set several parameters to tune the instance, e.g:

```
./vipdev.js create testing \
  --title "Site Title" \
  --php 7.3 \
  --wordpress 5.5.1 \
  --mu-plugins /home/code/vip-go-mu-plugins \
  --client-code /home/code/vip-wordpress-com
```

As you can see, you can choose to use your local clone of `mu-plugins` in case you want to develop on it (by default it will use a container that auto updates the repo). You can also choose the local path to the client code (by default it will use the `vip-go-skeleton` container).

You can also fetch all required data using the site id:

```
./vipdev.js create wpvip --site 1513
```

This will obtain the PHP and WordPress version from GOOP, and it will clone the git repo with the client code, putting it in `site-wpvip/clientcode`


### Upgrading an instance

After an instance has been created, you can upgrade some of its components. E.g:

```
./vipdev.js upgrade testing \
  --php 7.4 \
  --wordpress 5.5.1 \
  --mu-plugins auto
```

This will rebuild the app containers but without losing any data.


### To-Do

There are a few things that are needed before matching the functionalities of the current lando environment in `mu-dev`:

- Multisite support [DONE]
- Cron control
- Mu-plugins tests


### Container definitions

In the directory `docker` you can find the Dockerfiles for all containers used in this environment. They are pushed to the organization `wpvipdev` in dockerhub (all of them are based on open source projects, so they can be public)
