# Stakewars3_Challange14
Topic:Create, run, and modify the auto-backup node script.

I used Digital Ocean VPS as backup Node server.As you know before we installed our node on it and it is running (finished donwloadinh and synching-as you see the screenshot) also.

![image](https://user-images.githubusercontent.com/105415280/188285374-c50fa3c4-9375-4ddf-9ec5-ce11d1366d46.png)

I logged in as root. Firstly for ts if not installed  install moreutils with below command
```
apt install moreutils
```

then go to your home directory within below command
```
cd $home
```
make a file named as backp_sh
```
nano backp_sh
```

Copy the below commands in it , please be care that your directories depends on your installation
```
DATE=$(date +%Y-%m-%d-%H-%M)
DATADIR=/root/
BACKUPDIR=/root/near_${DATE}
mkdir $BACKUPDIR
systemctl stop neard
wait
echo "NEAR node was stopped" | ts
if [ -d "$BACKUPDIR" ]; then
        echo "Backup started" | ts
    cp -rf $DATADIR/.near/data/ ${BACKUPDIR}/
    echo "Backup completed" | ts
else
    echo $BACKUPDIR is not created.Check you commands
 exit 0
fi
systemctl start neard
echo "NEAR node was started" | ts
```

if everything is ok at the end you will see below screen after run it.

![image](https://user-images.githubusercontent.com/105415280/188288201-f924e609-4c71-4f80-9b06-f3fe16b53bcf.png)

and also check if the directory was created.

![image](https://user-images.githubusercontent.com/105415280/188288240-34737e23-3af0-40b1-b383-f244d4724558.png)

As you see we have directory "near_2022-09-04-00-07" let see inside it.

![image](https://user-images.githubusercontent.com/105415280/188288402-5d6ea09a-f802-40ba-a62a-2752a31ffb1d.png)


![image](https://user-images.githubusercontent.com/105415280/188288408-07d5e303-1800-408e-8c53-0fb53c305d75.png)


Now we finished backing the data. For automatic backup we will add it to crontab, add a line within the time for backup.

```
nano /etc/crontab
```
Below as you see my backup will be done everyday at 22.32.

![image](https://user-images.githubusercontent.com/105415280/188289558-c77e3d85-b034-4a45-ae1a-e3b30851951c.png)

Also we can see the new file created within the mentioned time at our crontab

![image](https://user-images.githubusercontent.com/105415280/188289581-7a000d5a-0b45-4d91-91eb-e03b4cd29b6b.png)


also we can check the log file 
```
nano backup.log
```

![image](https://user-images.githubusercontent.com/105415280/188289589-c1115a72-97b8-4d7b-9788-90b511c66a95.png)


Now for restoring the backup up data use below steps

```
cd $home
```
make a file named as rstr.sh
```
nano rstr.sh
```

Copy the below commands in it , please be care that your directories depends on your installation
```
DATE=$(date +%Y-%m-%d-%H-%M)
DATADIR=/root/
BACKUPDIR=/root/near_${DATE}

systemctl stop neard
wait
echo "NEAR node was stopped" | ts

    echo "Deleting olf files in data folder was started" | ts
    rm -R ${DATADIR}/data/*
    echo "Deleting completed" | ts
    echo "Restoring started" | ts
    cd ${DATADIR}
   cp -rf ${BACKUPDIR}/ $DATADIR/.near/data/ 
    echo "Restoring completed" | ts

sudo systemctl start neard
echo "NEAR node was started" | ts
```

Alos for restore you can use crontab again like above


