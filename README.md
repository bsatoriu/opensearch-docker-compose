# OpenSearch - Docker - Compose

## Cluster setup

Raise your host's ulimits for ElasticSearch to handle high I/O :

```bash
sudo sysctl -w vm.max_map_count=512000
# Persist this setting in `/etc/sysctl.conf` and execute `sysctl -p`
```

Now, we will generate the certificates for the cluster :

```bash
# You may want to edit the OPENDISTRO_DN variable first
bash generate-certs.sh
```

Start the cluster :

```bash
docker compose up -d
```

Wait about 30 seconds and run `securityadmin` to initialize the security plugin :

```bash
docker compose exec os01 bash -c "chmod +x plugins/opensearch-security/tools/securityadmin.sh && bash plugins/opensearch-security/tools/securityadmin.sh -cd config/opensearch-security -icl -nhnv -cacert config/certificates/ca/ca.pem -cert config/certificates/ca/admin.pem -key config/certificates/ca/admin.key -h localhost"
```

Access OpenSearch Dashboards through [https://localhost:5601](https://localhost:5601)

Default username is `admin` and password is `admin`
