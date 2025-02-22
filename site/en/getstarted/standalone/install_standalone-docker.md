---
id: install_standalone-docker.md
label: Docker Compose
related_key: Docker
order: 0
group: install_standalone-docker.md
summary: Learn how to install Milvus stanalone with Docker Compose.
---

# Install Milvus Standalone

{{fragments/installation_guide.md}}

{{tab}}

## Download an installation file

[Download](https://github.com/milvus-io/milvus/releases/download/v{{var.milvus_release_tag}}/milvus-standalone-docker-compose.yml) `milvus-standalone-docker-compose.yml` directly or with the following command, and save it as `docker-compose.yml`.

```
$ wget https://github.com/milvus-io/milvus/releases/download/v{{var.milvus_release_tag}}/milvus-standalone-docker-compose.yml -O docker-compose.yml
```

## Configure Milvus (optional)

[Download](https://raw.githubusercontent.com/milvus-io/milvus/v{{var.milvus_release_tag}}/configs/milvus.yaml) `milvus.yaml` directly or with the following command.

```
$ wget https://raw.githubusercontent.com/milvus-io/milvus/v{{var.milvus_release_tag}}/configs/milvus.yaml
```

Modify the configurations to suit your needs. See [Milvus Standalone System Configurations](configuration_standalone-basic.md) for more information.


In `docker-compose.yml`, map the local path to your `milvus.yaml` file onto the corresponding docker container path to the configuration file `/milvus/configs/milvus.yaml` under the `volumes` section.

```yaml
    volumes:
      - ${DOCKER_VOLUME_DIRECTORY:-.}/volumes/milvus:/var/lib/milvus
      - /local/path/to/your/file:/milvus/configs/milvus.yaml     # Map the local path to the container path
```

<div class="alert note">
Data is stored in the <code>volumes</code> folder according to the default configuration in <code>docker-compose.yml</code>. To change the folder to store data, edit <code>docker-compose.yml</code> or run <code>$ export DOCKER_VOLUME_DIRECTORY=</code>.
</div>

## Start Milvus

```shell
$ sudo docker-compose up -d
```

```text
Docker Compose is now in the Docker CLI, try `docker compose up`
Creating milvus-etcd  ... done
Creating milvus-minio ... done
Creating milvus-standalone ... done
```


Check the status of the containers.
```
$ sudo docker-compose ps
```

After Milvus standalone starts, three running docker containers appear including two dependencies and one Milvus service. 
```
      Name                     Command                  State                          Ports
----------------------------------------------------------------------------------------------------------------
milvus-etcd         etcd -listen-peer-urls=htt ...   Up (healthy)   2379/tcp, 2380/tcp
milvus-minio        /usr/bin/docker-entrypoint ...   Up (healthy)   9000/tcp
milvus-standalone   /tini -- milvus run standalone   Up             0.0.0.0:19530->19530/tcp,:::19530->19530/tcp
```

## Stop Milvus

To stop Milvus standalone, run <code>$ sudo docker-compose down</code>.

To delete data after stopping Milvus, run <code>$ sudo rm -rf  volumes</code>.

## What's next

Having installed Milvus, you can:

- Check [Hello Milvus](example_code.md) to run an example code with different SDKs to see what Milvus can do.

- Learn the basic operations of Milvus:
  - [Connect to Milvus server](manage_connection.md)
  - [Conduct a vector search](search.md)
  - [Conduct a hybrid search](hybridsearch.md)

- Explore [MilvusDM](migrate_overview.md), an open-source tool designed for importing and exporting data in Milvus.
- [Monitor Milvus with Prometheus](monitor.md).
