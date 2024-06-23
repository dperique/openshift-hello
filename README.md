# Hello World Demo On Openshift

Get [Openshift Local running](https://www.redhat.com/sysadmin/install-openshift-local).

Run these commands:

```bash
cd openshift
oc login -u developer https://api.crc.testing:6443
oc new-project hello-proj
oc apply -f buildconfig.yaml
oc apply -f imagestream.yaml
oc apply -f deploymentconfig.yaml
oc apply -f route.yaml
oc start-build hello-world --follow
```

To get credentials:

```bash
$ crc console --credentials
To login as a regular user, run 'oc login -u developer -p developer https://api.crc.testing:6443'.
To login as an admin, run 'oc login -u kubeadmin -p ... https://api.crc.testing:6443'
```

When creating a project, you get examples:

```bash
$ oc new-project hello-proj
Now using project "hello-proj" on server "https://api.crc.testing:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname
```

Notice this at the bottom of your `/etc/hosts` file:

```
# Added by CRC
127.0.0.1        api.crc.testing canary-openshift-ingress-canary.apps-crc.testing console-openshift-console.apps-crc.testing default-route-openshift-image-registry.apps-crc.testing downloads-openshift-console.apps-crc.testing oauth-openshift.apps-crc.testing hello-world-hello-proj.apps-crc.testing
# End of CRC section
```

After running the above commands:

```bash
$ oc get route hello-world
NAME          HOST/PORT                                 PATH   SERVICES      PORT       TERMINATION   WILDCARD
hello-world   hello-world-hello-proj.apps-crc.testing          hello-world   8080-tcp                 None

$ curl hello-world-hello-proj.apps-crc.testing:8080
"Hello, world!"
```