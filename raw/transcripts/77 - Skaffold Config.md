![[Pasted image 20260423011457.png]]
Skaffold Sync Update and Example Source Code

In the next lecture, Stephen will be finishing up the config YAML as well as showing a demo of how Skaffold would work in local development.

It is highly likely that you have just finished up the HTTPS configuration for your project which now includes an updated Ingress, Issuer, and Certificate manifests.

To avoid conflicts with these production resources, you will need to restructure your project files.

1. Create two additional folders along side the existing k8s directory called **k8s-dev** and **k8s-prod**

2. Move the **certificate.yaml**, **ingress-service.yaml,** and **issuer.yaml** files into **k8s-prod**.

3. Create a development-only **ingress-service.yaml** by copy-pasting the below code and placing into the **k8s-dev** directory:

4. apiVersion: networking.k8s.io/v1
5. # UPDATE API
6. kind: Ingress
7. metadata:
8.   name: ingress-service
9.   annotations:
10.     # REMOVE CLASSNAME ANNOTATION
11.     nginx.ingress.kubernetes.io/use-regex: "true"
12.     # ADD ANNOTATION
13.     nginx.ingress.kubernetes.io/rewrite-target: /$1
14.     # ADD ANNOTATION
15. spec:
16.   ingressClassName: nginx
17.   # ADD INGRESSCLASSNAME FIELD
18.   rules:
19.     - http:
20.         paths:
21.           - path: /?(.*)
22.             # UPDATE PATH
23.             pathType: ImplementationSpecific
24.             # ADD PATHTYPE
25.             backend:
26.               service:
27.                 # UPDATE SERVICE FIELDS
28.                 name: client-cluster-ip-service
29.                 port:
30.                   number: 3000
31.           - path: /api/?(.*)
32.             # UPDATE PATH
33.             pathType: ImplementationSpecific
34.             # ADD PATHTYPE
35.             backend:
36.               service:
37.                 # UPDATE SERVICE FIELDS
38.                 name: server-cluster-ip-service
39.                 port:
40.                   number: 5000

  

41. Update your skaffold.yaml to track both **k8s-dev/*** and **k8s/*** directories:

42. deploy:
43.   kubectl:
44.     manifests:
45.       - ./k8s/*
46.       - ./k8s-dev/*

  

47. Update the apiVersion:

48. apiVersion: skaffold/v2beta12

  

49. Update the globbing pattern syntax used for both the Client, Server, and Worker images.

Client image:

1.       sync:
2.         manual:
3.           - src: "src/**/*.js"
4.             dest: .
5.           - src: "src/**/*.css"
6.             dest: .
7.           - src: "src/**/*.html"
8.             dest: .

  

Server and Worker images:

1.       sync:
2.         manual:
3.           - src: "*.js"
4.             dest: .

  

The full updated skaffold.yaml can be found here (it's also included in the zip folder attached to this lecture):

1. apiVersion: skaffold/v2beta12
2. kind: Config
3. deploy:
4.   kubectl:
5.     manifests:
6.       - ./k8s/*
7.       - ./k8s-dev/*
8. build:
9.   local:
10.     push: false
11.   artifacts:
12.     - image: rallycoding/client-skaffold
13.       context: client
14.       docker:
15.         dockerfile: Dockerfile.dev
16.       sync:
17.         manual:
18.           - src: "src/**/*.js"
19.             dest: .
20.           - src: "src/**/*.css"
21.             dest: .
22.           - src: "src/**/*.html"
23.             dest: .
24.     - image: rallycoding/worker-skaffold
25.       context: worker
26.       docker:
27.         dockerfile: Dockerfile.dev
28.       sync:
29.         manual:
30.           - src: "*.js"
31.             dest: .
32.     - image: rallycoding/server-skaffold
33.       context: server
34.       docker:
35.         dockerfile: Dockerfile.dev
36.       sync:
37.         manual:
38.           - src: "*.js"
39.             dest: .

  

40. Replace my DockerHub username and image names with yours making sure the images in the k8s **server**, **worker**, and **client** deployment yaml manifests match what is shown in the Skaffold yaml. Also, make sure that you are using new development images and not production images.

41. Update your **client/Dockerfile.dev** to add the **CI=true** and **WDS_SOCKET_PORT=0** variables:

42. FROM node:16-alpine
43. ENV CI=true
44. ENV WDS_SOCKET_PORT=0

45. WORKDIR "/app"
46. COPY ./package.json ./
47. RUN npm install
48. COPY . .
49. CMD ["npm", "run", "start"]

  

Finally, run `skaffold dev` in your terminal (this may take several minutes to build initially since all four servers will be involved).

You should now be able to access the cluster at localhost if using Docker Desktop, or at your minikube IP.

The Skaffold syntax reference can be found here:

[https://skaffold.dev/docs/references/yaml/](https://skaffold.dev/docs/references/yaml/)