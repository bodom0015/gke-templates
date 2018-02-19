# gke-templates
Backup of slambert.org GKE YAML files

# Prerequisite
* GKE cluster running
* A static `loadBalancerIP` value

# Resume / Minecraft Status Portal
Create the service and controller:
```bash
kubectl create -f resume.yaml
```

You should now be able to access the web portal at http://loadBalancerIP/

# MongoDB
Create a new "Disk" in GCE to house your mongo data.

Create PV referencing new disk, then create the service and controller:
```bash
kubectl create -f mongo/mongo-pv.yaml
kubectl create -f mongo/mongo-svc.yaml -f mongo/mongo-controller.yaml
```

NOTE: this is a prerequisite for both UpStaging and Discord Notify Bot (see below)

## UpStaging
See https://github.com/bodom0015/upstaging

NOTE: this requires running MongoDB first (see above)

Fill in the configuration in the `upstaging.yaml` environment variables.

Then, create the service and controller:
```bash
kubectl create -f upstaging.yaml
```

You should now be able to access the upstaging web portal at http://loadBalancerIP:8080/upstaging/

## Discord Notify Bot
See https://github.com/bodom0015/discord-notify-bot

NOTE: this requires running MongoDB first (see above)

Fill in your bot's access token in the `dispatch-bot-rc.yaml` environment variables.

Then, create the controller:
```bash
kubectl create -f dispatch-bot-rc.yaml
```

You should see your bot join the Discord server, and it should now respond to commands via chat.

Send the bot `help` to get started!

# Minecraft + DynMap (Spigot Server)
See https://github.com/AshDevFr/docker-spigot

See https://hub.docker.com/r/ashdev/minecraft-spigot/

Create a new "Disk" in GCE to house your minecraft data.

Create PV referencing new disk, then create the service and controller:
```
kubectl create -f minecraft/minecraft-pv.yaml
kubectl create -f minecraft/minecraft-svc.yaml -f minecraft/minecraft-controller.yaml
```

You should now be able to access the Minecraft server's live map (DynMap) at http://loadBalancerIP:8123

You should also be able to join the Minecraft server by connecting to the loadBalancerIP. By default, the port used will be 25565.

NOTE: RCON is currently disabled
