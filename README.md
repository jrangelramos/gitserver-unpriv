# GIT Server for CI/CD

This project holds a lightweigth git server backed by NGINX and Alpine Linux. 
It is meant to be used by tests and CI/CD systems which needs a simple and ephemeral 
easy to use git server on their pipelines. 
This git server does not required priviledged resources and exposes the port 8080

## How to use it

You can quickly start a ephemeral git server using:
```
docker run -d -p 8080:8080 --name gitserver ghcr.io/jrangelramos/gitserver-unpriv:latest
```

Or if you prefer you can clone and build:

```
docker build -t gitserver .
```

Also you may specify a volume to store the repositories:

```
mkdir repository
docker run -d -p 8080:8080 --name gitserver \
  -v $(pwd)/repository:/var/www/git gitserver
```

### Creating repositories

The image has a utility `git-repo` you can use to create and delete repositories. The below example creates a *myrepo* test repository

```
docker exec gitserver git-repo create myrepo
```

And then you can clone and add content as below
```
git clone https://localhost:8080/myrepo.git
cd myrepo
echo "# My Repo" > README.md
git add README.md
git commit -m "initial commit"
git push
```

### Deleting repositories

You can use the same `git-repo` utility as below

```
docker exec gitserver git-repo delete myrepo
```
