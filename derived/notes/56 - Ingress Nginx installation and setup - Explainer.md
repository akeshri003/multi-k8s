# Ingress Nginx installation and setup Explainer

## Source

- Transcript: `raw/transcripts/56 - Ingress Nginx installation and setup.md`
- Wiki source page: [[56 - Ingress Nginx installation and setup]]

## Reference Visual

- External screenshot referenced by the source:
  ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-12-17_19-05-02-747c83600cdba303ec7b8a002e523251.png)

## What This Lesson Is Doing

This lesson is not really teaching a new Kubernetes object.

It is giving a practical setup instruction:

before the ingress config can do anything, you need the ingress-nginx controller installed in the cluster.

## The Simple Reading

If you are on Docker Desktop Kubernetes, the course does not want you to invent the install by hand.

It wants you to use the official ingress-nginx documentation.

Specifically:

- go to `https://kubernetes.github.io/ingress-nginx/deploy/#quick-start`
- find `If you don't Have Helm`
- copy the provided `kubectl apply` script
- run it in your terminal

## Why This Matters

Earlier lessons explained ingress conceptually:

- ingress config describes routing rules
- ingress controller makes those rules real

This lesson is the missing bridge between those ideas and actual local usage.

Without the controller installed, an ingress manifest would just be a config object with nothing acting on it.

## Practical Takeaway

The durable lesson here is:

- Docker Desktop local ingress setup is environment-specific
- for this course path, the source of truth for installation is the official ingress-nginx quick-start docs
- the next lesson will finally create the ingress manifest that uses that controller
