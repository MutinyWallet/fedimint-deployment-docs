# Fedimint Deployment Docs

Useful info for self-deploying fedimint software

## Instructions

The main part is in the `tls-download-mainnet.sh` script: 

```
curl -sSL https://raw.githubusercontent.com/tonygiorgio/fedimint/mainnet-deploy/docker/tls-download-mainnet.sh | bash
```

---

Some TBD instructions, not sure if it would be useful to put in an `.md` somewhere:

### ssh into your server

`ssh -i ~/.ssh/YOUR_ID root@<serverip>`

### update your system

```
sudo apt update
```

### install deps

```
sudo apt install docker-compose
sudo apt-get install jq
```

You might have to add your linux user to the docker group:

```
sudo usermod -aG docker YOUR_USER_HERE
```

log out and back in after you do that. You may not have to do this step, but if you get docker permission errors at the end, you'll have to do this and then log out and run the script again.

### open up the correct ports

```
sudo ufw allow 443
sudo ufw allow 8173
sudo ufw allow 9735
sudo ufw enable
sudo systemctl enable ufw
```

### run the script

```
curl -sSL https://raw.githubusercontent.com/tonygiorgio/fedimint/mainnet-deploy/docker/tls-download-mainnet.sh | bash
```

### follow the script instructions

1. say no when it asks you if you want to run an LND gateway

### after it's done

if you need to restart your server you can start up fedimint again by running
```
docker-compose up -d
```

to see the status 
```
docker-compose ps
```

to see some logs
```
docker-compose logs -f
```

### Guardian Setup

Now go to your guardian UI and start the setup flow

It should be something like: guardian-ui.fedimint.yourdomain.com

If you are just creating a one person test, go to "solo", if you are doing this with multiple people, then one person will start with "leader" and start the process. They will create the federation name and general federation info. Once the leader has an invite URL, then the rest should go through the "follower" flow. The followers will create their member info and then there's a confirmation stage where everyone confirms their session IDs together. Then the federation is set up. 

### Backups

A backup should be done after a federation was started. These initial files will ensure that you can recover later, with the help of the other federation members to resync the missing state. 

```
scp -r <username>@<ip address of server>:/var/lib/docker/volumes <whatever you want to call the backup file>
```
