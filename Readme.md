
Username:         user

#Installation

sudo yum -y install rsync lsyncd

ssh-keygen

sudo ssh-copy-id user@172.16.1.172 ; ssh-copy-id user@172.17.1.172

#Test

ssh user@172.16.1.172

#Permission 

sudo chmod -R 0777 /vol/data/tmp/


#Configuration

sudo vi /etc/lsyncd.conf


settings  {

        logfile = "/var/log/lsyncd/lsyncd.log",
        statusFile = "/var/log/lsyncd/lsyncd.status",
        nodaemon   = false,
        statusInterval = 1,
        insist = true
}

sync {

        default.rsync,
        source = "/vol/data/tmp",
        target = "user@172.16.1.172:/vol/data/tmp",
        delete = false,
        delay = 1,

}

sync {

        default.rsync,
        source = "/vol/data/tmp",
        target = "user@172.17.1.172:/vol/data/tmp",
        delete = false,
        delay = 1,

}




#Restart

sudo systemctl restart lsyncd




#Testing 

sudo rsync -avzh /vol/data/tmp/ user@172.17.1.172:/vol/data/tmp/
