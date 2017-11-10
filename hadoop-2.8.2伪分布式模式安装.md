# hadoop-2.8.2安装
## 参考
http://hadoop.apache.org/docs/r2.8.2/hadoop-project-dist/hadoop-common/SingleCluster.html
</br>
https://www.cnblogs.com/wcwen1990/p/6729413.html

## 环境
* [virtualbox](https://www.virtualbox.org/wiki/Linux_Downloads)
* [centos6.8_64](http://vault.centos.org/6.8/isos/x86_64/)
* [jdk1.7+(必须)](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html)</br>
      [hadoop与java版本对应关系](https://wiki.apache.org/hadoop/HadoopJavaVersions)
* [hadoop-2.8.2](https://archive.apache.org/dist/hadoop/common/hadoop-2.8.2/)

## 安装jdk
## 安装ssh和rsync
>yum -y install openssh-server openssh-clients</br>
 yum install rsync

## 配置当前用户免密登录

## 安装hadoop-2.8.2.tar.gz
### 解压
> tar zxvf hadoop-2.8.2.tar.gz -C /opt/
### 修改配置,3个文件（core-site.xml、hdfs-site.xml、hadoop-env.sh）
> cd /opt/hadoop-2.8.2</br>
vi etc/hadoop/core-site.xml
>> &lt;configuration&gt;</br>
        &lt;property&gt;</br>
                &lt;name&gt;fs.defaultFS&lt;/name&gt;</br>
                &lt;value&gt;hdfs://localhost:9000&lt;/value&gt;</br>
        &lt;/property&gt;</br>
        &lt;property&gt;</br>
                &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;</br>
                &lt;value&gt;/opt/hadoop-2.8.2/tmp&lt;/value&gt;</br>
        &lt;/property&gt;</br>
        &lt;property&gt;</br>
                &lt;name&gt;fs.s3.block.size&lt;/name&gt;</br>
                &lt;value&gt;8388608&lt;/value&gt;</br>
                &lt;description&gt;Block size to use when writing files to S3.&lt;/description&gt;</br>
        &lt;/property&gt;</br>
&lt;/configuration&gt;</br>

  
  
vi etc/hadoop/hdfs-site.xml
>> &lt;configuration&gt;</br>
        &lt;property&gt;</br>
                &lt;name&gt;dfs.replication&lt;/name&gt;</br>
                &lt;value&gt;1&lt;/value&gt;</br>
        &lt;/property&gt;</br>
        &lt;property&gt;</br>
                &lt;name&gt;dfs.namenode.name.dir&lt;/name&gt;</br>
                &lt;value&gt;file:/opt/hadoop-2.8.2/data/dfs/namenode&lt;/value&gt;</br>
        &lt;/property&gt;</br>
        &lt;property&gt;</br>
                &lt;name&gt;dfs.datanode.data.dir&lt;/name&gt;</br>
                &lt;value&gt;file:/opt/hadoop-2.8.2/data/dfs/datanode&lt;/value&gt;</br>
        &lt;/property&gt;</br>
&lt;/configuration&gt;</br>  

> vi bin/hadoop-env.sh
>> export JAVA_HOME=/usr/local/jdk

### 格式化hdfs
bin/hdfs namenode -format

### 启动
/opt/hadoop-2.8.2/sbin/start-all.sh

### 验证
* 用jps可以看到5个进程(DataNode、SecondaryNameNode、NameNode、ResourceManager、NodeManager)
* http://localhost:50070/
* http://localhost:8088/
