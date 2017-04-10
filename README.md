### openshift-nodejs-getting-started
#OpenShift with NodeJS - Getting Started

Based on 'Getting Started with NodeJS on OpenShift Online Next Gen' at https://blog.openshift.com/getting-started-nodejs-oso3/

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

