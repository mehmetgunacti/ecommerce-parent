* Kubernetes and Docker don’t trust self-signed certificates by default.
* Adding the certificate to /etc/docker/certs.d/ ensures that Docker and containerd recognize it as a valid registry.

```bash
# create certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout ecommerce-registry.key \
  -out ecommerce-registry.crt \
  -subj "/CN=registry.ecommerce.local" \
  -addext "subjectAltName = DNS:registry.ecommerce.local"

# run registry
docker run -d \
  --restart=always \
  --name registry \
  -p 5000:5000 \
  -v $(pwd)/ecommerce-registry.crt:/certs/registry.crt \
  -v $(pwd)/ecommerce-registry.key:/certs/registry.key \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/registry.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/registry.key \
  registry:2


# copy certs
sudo mkdir -p /etc/docker/certs.d/registry.ecommerce.local:5000/
sudo cp ecommerce-registry.crt /etc/docker/certs.d/registry.ecommerce.local:5000/ca.crt

# sudo systemctl restart docker
sudo systemctl restart containerd
sudo systemctl daemon-reload

sudo cp ecommerce-registry.crt /usr/local/share/ca-certificates/ecommerce-registry.crt
sudo update-ca-certificates

# Then restart the cluster nodes as needed.

# Tests
docker pull registry.ecommerce.local:5000/ecommerce-jenkins-kubectl:latest
# re-apply deployment
```