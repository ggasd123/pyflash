# python flask example

Dockerfile and manifests for a python Flask App using Gunicorn.

Use:

    docker build -t cloudgenius/pyflask -f Dockerfile.alpine .

    docker login

    docker push cloudgenius/pyflask

    docker run -d -p 5000:5000 cloudgenius/pyflask

Test locally:

    open http://localhost:5000


Deploy:

    kubectl apply -f manifest.yaml

Check it out:

    open https://pyflask.boeing.internal.cloudgenius.app
