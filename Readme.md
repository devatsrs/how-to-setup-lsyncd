#Readme

Here I will explain commands to setup and configure auto folder/directory sync across servers.

#Servers

 172.16.1.172

 172.17.1.172

 172.18.1.172


Setting up following commands for server 172.18.1.172.


Username:         user

#Installation

sudo yum -y install rsync lsyncd

#Permissions 

ssh-keygen

sudo ssh-copy-id user@172.16.1.172 

sudo ssh-copy-id user@172.17.1.172

sudo chmod -R 0777 /vol/data/tmp/


#Test

sudo ssh user@172.16.1.172






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
        rsync = { perms=true, chmod = "775", _extra = { "--omit-dir-times" }  }


}

sync {

        default.rsync,
        source = "/vol/data/tmp",
        target = "user@172.17.1.172:/vol/data/tmp",
        delete = false,
        delay = 1,
        rsync = { perms=true, chmod = "775", _extra = { "--omit-dir-times" }  }


}




#Restart

sudo systemctl restart lsyncd




#Testing 

sudo rsync -avzh /vol/data/tmp/ user@172.17.1.172:/vol/data/tmp/


#Reference

https://axkibe.github.io/lsyncd/manual/config/layer4/
