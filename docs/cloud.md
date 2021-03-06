# Use With Cloud Storage

## Upload malware to a [minio](https://minio.io/) server

Start the `minio` server

```bash
$ docker run -d --name minio \
             -p 9000:9000 \
             -e MINIO_ACCESS_KEY=admin \
             -e MINIO_SECRET_KEY=password \
             minio/minio server /data
```

Download malware into the `malice` bucket of the `minio` instance

```bash
$ docker run --rm -it --link minio \
         malice/get-mauled \
         --store-url minio:9000 \
         --store-id admin \
         --store-key password \
         download --password infected \
         https://github.com/ytisf/theZoo/raw/master/malwares/Binaries/Duqu2/Duqu2.zip
```

Open [http://localhost:9000/minio/malice/](http://localhost:9000/minio/malice/) to see the files _(creds:**admin/password**)_

![minio](https://raw.githubusercontent.com/malice-plugins/get-mauled/master/docs/minio.png)

## Upload malware to DigitalOcean [Spaces](https://www.digitalocean.com/docs/spaces/)

### Create a Space

![create](https://raw.githubusercontent.com/malice-plugins/get-mauled/master/docs/do-create.png)

### Get Creds

![creds](https://raw.githubusercontent.com/malice-plugins/get-mauled/master/docs/do-creds.png)

### Upload malware into the `malice` Space

```bash
$ docker run --rm -it \
         malice/get-mauled \
         --store-url malice.nyc3.digitaloceanspaces.com \
         --store-id $DO_SPACE_KEY \
         --store-key $DO_SPACE_SECRET \
         --store-tls \
         download --password infected \
         https://github.com/ytisf/theZoo/raw/master/malwares/Binaries/Duqu2/Duqu2.zip

INFO[0000] Successfully created bucket malice
INFO[0000] Downloading file: https://github.com/ytisf/theZoo/raw/master/malwares/Binaries/Duqu2/Duqu2.zip
INFO[0007] malware successfully sent to cloud storage    count=22 total_size="921 kB"
```

![files](https://raw.githubusercontent.com/malice-plugins/get-mauled/master/docs/do-files.png)
