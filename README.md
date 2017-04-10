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

***NOTE***: PillarsJS - is a modular and scalable framework for web development in Node.js. See also http://pillarsjs.com

The command output should look something like:

```javascript
--> Found image fc2b55e (6 weeks old) in image stream "openshift/nodejs" under tag "4" for "nodejs"

    Node.js 4
    ---------
    Platform for building and running Node.js 4 applications

    Tags: builder, nodejs, nodejs4

    * A source build using source code from http://github.com/OpenShiftDemos/pillar-base will be created
      * The resulting image will be pushed to image stream "pillar-base:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "pillar-base"
    * Port 8080/tcp will be load balanced by service "pillar-base"
      * Other containers can access this service through the hostname "pillar-base"

--> Creating resources ...
    imagestream "pillar-base" created
    buildconfig "pillar-base" created
    deploymentconfig "pillar-base" created
    service "pillar-base" created
--> Success
    Build scheduled, use 'oc logs -f bc/pillar-base' to track its progress.
    Run 'oc status' to view your app.
```

Several new kubernetes objects (service, deploymentconfig, imagestream, buildconfig) have been successfully created, as shown in the output.

Track the progress with the following command:

```javascript
oc logs -f bc/pillar-base
```

It will output something like:

```javascript
...
├── escape-html@1.0.3
├── encodeurl@1.0.1
├── parseurl@1.3.1
└── send@0.15.1 (destroy@1.0.4, ms@0.7.2, etag@1.8.0, fresh@0.5.0, range-parser@1.2.0, statuses@1.3.1, mime@1.3.4, depd@1.1.0, de
bug@2.6.1, on-finished@2.3.0, http-errors@1.6.1)

gulp@3.9.1 node_modules/gulp
├── interpret@1.0.2
├── pretty-hrtime@1.0.3
├── deprecated@0.0.1
├── archy@1.0.0
├── minimist@1.2.0
├── tildify@1.2.0 (os-homedir@1.0.2)
├── semver@4.3.6
├── v8flags@2.0.12 (user-home@1.1.1)
├── chalk@1.1.3 (escape-string-regexp@1.0.5, supports-color@2.0.0, ansi-styles@2.2.1, has-ansi@2.0.0, strip-ansi@3.0.1)
├── orchestrator@0.3.8 (sequencify@0.0.7, stream-consume@0.1.0, end-of-stream@0.1.5)
├── gulp-util@3.0.8 (array-differ@1.0.0, array-uniq@1.0.3, beeper@1.1.1, object-assign@3.0.0, lodash._reescape@3.0.0, lodash._ree
valuate@3.0.0, lodash._reinterpolate@3.0.0, dateformat@2.0.0, replace-ext@0.0.1, has-gulplog@0.1.0, fancy-log@1.3.0, vinyl@0.5.3,
 gulplog@1.0.0, lodash.template@3.6.2, through2@2.0.3, multipipe@0.1.2)
├── vinyl-fs@0.3.14 (strip-bom@1.0.0, vinyl@0.4.6, graceful-fs@3.0.11, defaults@1.0.3, through2@0.6.5, mkdirp@0.5.1, glob-stream@
3.1.18, glob-watcher@0.0.6)
└── liftoff@2.3.0 (lodash.isplainobject@4.0.6, lodash.isstring@4.0.1, lodash.mapvalues@4.6.0, rechoir@0.6.2, extend@3.0.0, flagge
d-respawn@0.3.2, resolve@1.3.2, fined@1.0.2, findup-sync@0.4.3)

tap@5.8.0 node_modules/tap
├── stack-utils@0.4.0
├── clean-yaml-object@0.1.0
├── opener@1.4.3
├── supports-color@1.3.1
├── isexe@1.1.2
├── only-shallow@1.2.0
├── deeper@2.1.0
├── tmatch@2.0.1
├── tap-parser@1.3.2 (inherits@2.0.3, events-to-array@1.0.2)
├── signal-exit@2.1.2
├── readable-stream@2.2.9 (inherits@2.0.3, buffer-shims@1.0.0, string_decoder@1.0.0, process-nextick-args@1.0.7, util-deprecate@1
.0.2, core-util-is@1.0.2, isarray@1.0.0)
├── bluebird@3.5.0
├── glob@7.1.1 (inherits@2.0.3, path-is-absolute@1.0.1, fs.realpath@1.0.0, once@1.4.0, inflight@1.0.6, minimatch@3.0.3)
├── foreground-child@1.5.6 (signal-exit@3.0.2, cross-spawn@4.0.2)
├── tap-mocha-reporter@0.0.27 (escape-string-regexp@1.0.5, diff@1.4.0, color-support@1.1.2, readable-stream@1.1.14, unicode-lengt
h@1.0.3, debug@2.6.3)
├── js-yaml@3.8.3 (esprima@3.1.3, argparse@1.0.9)
├── codecov.io@0.1.6 (request@2.42.0, urlgrey@0.4.0)
├── coveralls@2.13.0 (log-driver@1.2.5, lcov-parse@0.0.10, minimist@1.2.0, js-yaml@3.6.1, request@2.79.0)
└── nyc@6.6.1

Pushing image 172.30.47.227:5000/totaljs-001/pillar-base:latest ...
Pushed 0/5 layers, 1% complete
Pushed 1/5 layers, 25% complete
Pushed 2/5 layers, 65% complete
Pushed 3/5 layers, 74% complete
Pushed 4/5 layers, 98% complete
Pushed 5/5 layers, 100% complete
Push successful
```

Run the following command to view the status of your app:

```javascript
oc status
```

It will output something like:

```javascript
In project TotalJS - 001 (totaljs-001) on server https://api.preview.openshift.com:443

http://ce-nodejs-ex001-totaljs-001.44fs.preview.openshiftapps.com to pod port 8080-tcp (svc/ce-nodejs-ex001)
  dc/ce-nodejs-ex001 deploys istag/ce-nodejs-ex001:latest <-
    bc/ce-nodejs-ex001 source builds https://github.com/CreationsEcosystem/nodejs-ex.git#master on openshift/nodejs:4
    deployment #5 deployed 4 days ago - 0 pods
    deployment #4 deployed 4 days ago

svc/mongodb - 172.30.146.136:27017
  dc/mongodb deploys openshift/mongodb:3.2
    deployment #1 deployed 2 days ago - 1 pod

http://nodejs-mongo-persistent-totaljs-001.44fs.preview.openshiftapps.com (svc/nodejs-mongo-persistent)
  dc/nodejs-mongo-persistent deploys istag/nodejs-mongo-persistent:latest <-
    bc/nodejs-mongo-persistent source builds https://github.com/willem-vanheemstrasystems/nodejs-ex.git on openshift/nodejs:4
    deployment #1 deployed 2 days ago - 1 pod

http://openshift-totaljs-mongodb-totaljs-001.44fs.preview.openshiftapps.com (svc/openshift-totaljs-mongodb)
  dc/openshift-totaljs-mongodb deploys istag/openshift-totaljs-mongodb:latest <-
    bc/openshift-totaljs-mongodb source builds https://github.com/willem-vanheemstrasystems/openshift-totaljs-mongodb.git on open
shift/nodejs:4
      build #2 failed 2 days ago - 386e011: Add Applied Parameter Values (Willem van Heemstra <willem@sumedia.nl>)
    deployment #1 deployed 2 days ago - 1 pod

svc/pillar-base - 172.30.94.15:8080
  dc/pillar-base deploys istag/pillar-base:latest <-
    bc/pillar-base source builds http://github.com/OpenShiftDemos/pillar-base on openshift/nodejs:4
    deployment #1 deployed 5 minutes ago - 1 pod

9 warnings identified, use 'oc status -v' to see details.
```

With ```oc status -v``` it shows the 9 warnings:

```javascript
Warnings:
  * pod/ce-nodejs-ex001-4-build has no liveness probe to verify pods are still running.
    try: oc set probe pod/ce-nodejs-ex001-4-build --liveness ...
  * pod/ce-nodejs-ex001-5-build has no liveness probe to verify pods are still running.
    try: oc set probe pod/ce-nodejs-ex001-5-build --liveness ...
  * pod/openshift-totaljs-mongodb-1-build has no liveness probe to verify pods are still running.
    try: oc set probe pod/openshift-totaljs-mongodb-1-build --liveness ...
  * pod/openshift-totaljs-mongodb-2-build has no liveness probe to verify pods are still running.
    try: oc set probe pod/openshift-totaljs-mongodb-2-build --liveness ...
  * pod/pillar-base-1-build has no liveness probe to verify pods are still running.
    try: oc set probe pod/pillar-base-1-build --liveness ...
  * dc/ce-nodejs-ex001 has no readiness probe to verify pods are ready to accept traffic or ensure deployment is successful.
    try: oc set probe dc/ce-nodejs-ex001 --readiness ...
  * dc/ce-nodejs-ex001 has no liveness probe to verify pods are still running.
    try: oc set probe dc/ce-nodejs-ex001 --liveness ...
  * dc/pillar-base has no readiness probe to verify pods are ready to accept traffic or ensure deployment is successful.
    try: oc set probe dc/pillar-base --readiness ...
  * dc/pillar-base has no liveness probe to verify pods are still running.
    try: oc set probe dc/pillar-base --liveness ...

View details with 'oc describe <resource>/<name>' or list everything with 'oc get all'.
```

To make your new Kubernetes service available via an http route, run the following command after updating it to include your service name as the last argument:

