# fiber-http helm chart
This is a ready-made Helm Chart that will install fiber-http, with Opsani preinstalled and ready to go.  This includes:

* fiber-http running an envoy sidecar.
* k6 running in it's own pod for load generation.
* The Opsani Servo responsible for scraping metrics from the sidebar to send to the Opsani Optmizer backend (SaaS)

## Prerequisites

* You have a kubernetes cluster ready to go (created via the Makefile from https://github.com/opsani/dev-trial)
* You have an Opsani Backend created with a token in hand.

## Installation

Edit `values.yml` to include your Opsani specific settings (you shouldn't need to touch anything else here) - add your account name, application name your token in base64 format: `echo <token> | base64` 

```yaml
# values.yaml

# opsani - Opsani specific settings
opsani:
  account: <account_name>
  application: <application_name>
  # token - opsani token in base64 format
  token:
  namespace: default
```

Then run the following from the charts directory:

`helm install fiber-http ./fiber-http`

Example:
```
❯ helm install fiber-http ./fiber-http
NAME: fiber-http
LAST DEPLOYED: Mon Dec 28 17:18:38 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:

  .oooooo.                                              o8o
 d8P'  `Y8b                                             `"'
888      888 oo.ooooo.   .oooo.o  .oooo.   ooo. .oo.   oooo
888      888  888' `88b d88(  "8 `P  )88b  `888P"Y88b  `888
888      888  888   888 `"Y88b.   .oP"888   888   888   888
`88b    d88'  888   888 o.  )88b d8(  888   888   888   888
 `Y8bood8P'   888bod8P' 8""888P' `Y888""8o o888o o888o o888o
              888
             o888o
Dev // Cloud Native Optimization for Kubernetes application

fiber-http has been installed alongside an envoy sidecar with a k6
pod (for load generation) and an Opsani Servo pod.

You may view your application in the Opsani Console at:

https://console.opsani.com/accounts/dev.opsani.com/applications/ben-fiber-http-helm
```

## Wrap up

We should now have our main application (fiber-http) runnning with with an envoy sidebar, a k6 pod generating load, an Opsani Servo pod and within a few minutes we'll expect to also see our application canary deployed:

```
❯ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
fiber-http-6cb4d98c9f-dx2tv   2/2     Running   0          46m
fiber-http-canary             2/2     Running   0          73s
k6-5c4875c44d-lp7gp           1/1     Running   0          46m
servo-d99854c9d-n9w2v         2/2     Running   0          46m
```

You can access your application via the Opsani Console from:

 `https://console.opsani.com/accounts/<account_name>/applications/<application_name>`
