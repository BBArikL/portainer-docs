# Can I downgrade from Portainer Business to Portainer CE?

Yes, you can downgrade from Portainer Business Edition (BE) to Portainer Community Edition (CE). This can be for any reason — including if you decide not to purchase a full license after your free Portainer Business Edition trial period has ended.

{% hint style="warning" %}
The downgrade process only works if you upgraded from CE to BE (the process points you back to the CE database which doesn't get deleted when BE is installed). If you did not come from a CE instance, you won't be able to downgrade. You will need to do a full install of CE.
{% endhint %}

While you shouldn't experience any data loss, we recommend [backing up your Portainer data](../../admin/settings/#backup-portainer) before downgrading.

If you have a running instance of Portainer Business and want to downgrade to Portainer CE, follow the instructions below.

## On Docker <a href="#on-docker" id="on-docker"></a>

### Step 1: Shut down the existing Portainer Business instance <a href="#shutdown-the-existing-portainer-business-instance" id="shutdown-the-existing-portainer-business-instance"></a>

Make sure that the Portainer Business instance is stopped before attempting any of the other steps.

In Docker Standalone:

```
 docker stop portainer
```

Inside a Swarm environment, we recommend scaling down the Portainer service to 0 replicas:

```
 docker service scale portainer=0
```

### Step 2: Backup your data <a href="#backup-your-data" id="backup-your-data"></a>

First make sure to create a copy of the Portainer data volume.

Next, use the following command to backup the Portainer Business instance data, remembering to update the command to match the name of your Portainer container. This will create a `backup.tar` file in your current folder containing the Portainer Business instance data backup:

```
 docker run --rm --volumes-from portainer -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /data
```

### Step 3: Downgrade the Portainer Business database <a href="#downgrade-the-portainer-business-database" id="downgrade-the-portainer-business-database"></a>

Use the following command to downgrade the Portainer database:

```
 docker run -it --name portainer-database-rollback -v portainer_data:/data portainer/portainer-ee:latest --rollback-to-ce
```

### Step 4: Redeploy a Portainer CE instance <a href="#redeploy-a-portainer-ce-instance" id="redeploy-a-portainer-ce-instance"></a>

After downgrading the database, you can redeploy Portainer CE and re-use the existing Portainer Business data.

