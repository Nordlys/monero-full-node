# monero-full-node

docker image to run a monero full network node

# October 2018: Breaking Change
**warning**  
for improved security the new images will run the monero daemon under it's own user and not as root anymore!
If you simply upgrade without following the next steps you will run into this error:  
`WARN 	blockchain.db.lmdb	src/blockchain_db/lmdb/db_lmdb.cpp:75	Failed to open lmdb environment: Permission denied`

this can be fixed with the following steps  

* stop and remove the current container: `docker stop monerod && docker rm monerod`
* change the owner of the volume to monero user `docker run -v xmrchain:/home/monero/.bitmonero -t --rm --name=monerod -u root --entrypoint=/bin/chown nordlys/monero-full-node -R monero:monero .bitmonero`
* start the container `docker run -tid --restart=always -v xmrchain:/home/monero/.bitmonero -p 18080:18080 -p 18081:18081 --name=monerod nordlys/monero-full-node`

**Hint:** keep in mind that you have to adapt your volume bindings to your own configuration e.g. if you followed the older version of this readme you have to use: `-v /var/data/blockchain-xmr:/home/monero/.bitmonero` instead of `-v xmrchain:/home/monero/.bitmonero`

# Usage

**first start:**  
you need to change the permission of the mounted volume to allow the monero user inside the container to write the blockain in the volume. To do this, you have to mount the volume where you want to store the blockchain to the container and chown that path to the monero user. e.g.

`docker run -v xmrchain:/home/monero/.bitmonero -t --rm --name=monerod -u root --entrypoint=/bin/chown gitnordlys/monero-full-node -R monero:monero .bitmonero`

you have to do this only once before first start.

After this, you can start the container with e.g.

`docker run -tid --restart=always -v xmrchain:/home/monero/.bitmonero -p 18080:18080 -p 18081:18081 --name=monerod nordlys/monero-full-node`

## Updates
Manual Way
```
docker stop monerod
docker rm monerod
docker pull nordlys/monero-full-node
```
Then launch using the "how to use" command above

