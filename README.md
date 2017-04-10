### openshift-nodejs-getting-started
#OpenShift with NodeJS - Getting Started

Based on 'Getting Started with NodeJS on OpenShift Online Next Gen' at https://blog.openshift.com/getting-started-nodejs-oso3/

To follow along, you’ll need access to an OpenShift account, and a development environment with nodejs, git, and a copy of the “oc” command line tool.

## Login to OpenShift Online Next Gen

Start a Terminal session, type:

```javascript
oc login
```

You will be prompted as follows:

```javascript
Login failed (401 Unauthorized)
You must obtain an API token by visiting https://api.preview.openshift.com/oauth/token/request
```

If you follow the link, after logging in, your API token will be displayed:

Your API token is

2Oqm_6ypYIcpu8lOJN0LbYLJ-PhOJ8g9xpDvxpzFOTP

Log in with this token

```javascript
oc login --token=2Oqm_6ypYIcpu8lOJN0LbYLJ-PhOJ8g9xpDvxpzFOTP --server=https://api.preview.openshift.com
```

Use this token directly against the API

```javascript
curl -H "Authorization: Bearer 2Oqm_6ypYIcpu8lOJN0LbYLJ-PhOJ8g9xpDvxpzFOTP" "https://api.preview.openshift.com/oapi/v1/users/~"
```

Now try to login again as follows:

```javascript
oc login --token=2Oqm_6ypYIcpu8lOJN0LbYLJ-PhOJ8g9xpDvxpzFOTP --server=https://api.preview.openshift.com
```

You will be prompted as follows:

```javascript
Logged into "https://api.preview.openshift.com:443" as "willem-vanheemstrasystems" using the token provided.

You have one project on this server: "totaljs-001"

Using project "totaljs-001".
```

## Build and Deploy Images from the OpenShift Command Line Interface (CLI) Tool

Adapting your NodeJS source code to run on in a container can usually be accomplished in two easy steps:

1. Include a package.json file with valid "dependencies" and "scripts" sections – allowing builds to be started with “npm install”, and webservers to be initialized via “npm start”

2. Your source repo should launch a single web process that listens on port “8080” in order to connect to the Kubernetes “Service” (or load-balancer)

Schedule a new build and deployment, adding source code from “REPO_NAME” to a new image layer on top of “BASE_IMAGE”:

```javascript
oc new-app BASE_IMAGE~REPO_NAME
```

For example, to schedule a build and deployment, layering the [pillar-base example repo](http://github.com/OpenShiftDemos/pillar-base) on top of the [system-provided nodejs base image](https://blog.openshift.com/getting-started-nodejs-oso3/#Base_Images), run:

```javascript
oc new-app nodejs~http://github.com/OpenShiftDemos/pillar-base
```



