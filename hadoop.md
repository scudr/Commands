
## Hadoop Installation


## Prerequisite: Java
Make sure you have Java installed on your server, as Hadoop requires Java. You can check this by running:
```bash
java -version
readlink -f $(which java)
```

## Prerequisite: Check SSH
```bash
ssh
```

## Download and Extract Hadoop

```bash
wget --no-check-certificate https://dlcdn.apache.org/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
```
Then, extract the downloaded file with:
```bash
tar -xvf hadoop-3.3.0.tar.gz
```

## Move Hadoop File
```bash
sudo mv hadoop-3.3.0 /usr/local/hadoop
```

## Set Up Environment Variables
```bash
nano ~/.bashrc
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
```
Then, apply changes:
```bash
source ~/.bashrc
```

## Edit Hadoop Configuration Files
```bash
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```
or
1. First, navigate to the Hadoop etc/hadoop directory:

```bash
cd /usr/local/hadoop/etc/hadoop
nano hadoop-env.sh
```

In the nano editor, you should see the content of the *hadoop-env.sh* file. Scroll to the bottom of the file (or find an appropriate spot), and add the following lines (or modify if these variables already exist):
```bash
export HDFS_NAMENODE_USER="hadoop"
export HDFS_DATANODE_USER="hadoop"
export HDFS_SECONDARYNAMENODE_USER="hadoop"
export YARN_RESOURCEMANAGER_USER="yarn"
export YARN_NODEMANAGER_USER="yarn"
```
In the same *hadoop-env.sh* add:
```bash
export JAVA_HOME=/path/to/jdk
```
## ERRORS: 
```bash
start-dfs.sh
```
Starting namenodes on [localhost]
localhost: Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
localhost: Permission denied (publickey,password).

The *"Permission denied (publickey,password)"* error indicates that SSH is not set up correctly on your local machine. Hadoop requires password-less SSH access to manage its nodes.

## First, check if you already have an SSH key pair.
This key pair is typically stored in ~/.ssh/id_rsa.pub (public key) and ~/.ssh/id_rsa (private key). You can do this by running:
```bash
cat ~/.ssh/id_rsa.pub
```

## generate a new SSH key pair, run:
```bash
ssh-keygen -t rsa -P ""
```

## add your public key to the list of authorized keys
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
## test the setup by running:
```bash
ssh localhost
```

## generate a new key pair without a passphrase using the following steps:
First, backup your current SSH keys:

```bash
cp ~/.ssh/id_rsa ~/.ssh/id_rsa.backup
cp ~/.ssh/id_rsa.pub ~/.ssh/id_rsa.pub.backup
```
remove the current SSH keys:
```bash
rm ~/.ssh/id_rsa
rm ~/.ssh/id_rsa.pub
```
Generate a new SSH key pair without a passphrase:
```bash
ssh-keygen -t rsa -P ""
```
add your new public key to the authorized_keys file:
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
## test the setup by running:
```bash
ssh localhost
```

In $HADOOP_HOME/etc/hadoop/core-site.xml, add:
```xml
<configuration>
<property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>

```
In $HADOOP_HOME/etc/hadoop/hdfs-site.xml, add:
```xml
<configuration>
<property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>file:///usr/local/hadoop/hadoop_data/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>file:///usr/local/hadoop/hadoop_data/hdfs/datanode</value>
    </property>
</configuration>
```
In */usr/local/hadoop/etc/hadoop/yarn-site.xml* configuration file for YARN:
```xml
<configuration>
  <!-- YARN ResourceManager hostname -->
  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>localhost</value>
  </property>

  <!-- YARN ResourceManager web UI port -->
  <property>
    <name>yarn.resourcemanager.webapp.address</name>
    <value>0.0.0.0:8088</value>
  </property>
</configuration>
```

## Format HDFS
```bash
hdfs namenode -format
```

## Start Hadoop Services
```bash
start-dfs.sh
start-yarn.sh
```

You can access the web interface for Hadoop by going to *http://localhost:9870/* for the HDFS overview and *http://localhost:8088/* for the YARN overview. 