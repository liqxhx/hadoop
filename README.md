# hadoop1.1.2集群环境搭建

## 环境
* [virtualbox](https://www.virtualbox.org/wiki/Linux_Downloads)
* [centos6.8_64](http://vault.centos.org/6.8/isos/x86_64/)
* [javase6](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase6-419409.html)<br>
* [hadoop-1.1.2](https://archive.apache.org/dist/hadoop/common/hadoop-1.1.2/)<br>

## 配置服务器(为了方便都是root用户操作)
### 更新yum源
[参考这里](http://mirrors.aliyun.com/help/centos?spm=5176.bbsr150321.0.0.d6ykiD)

### 安装ssh
> yum -y install openssh-server openssh-clients
### 关闭防火墙
> /etc/init.d/iptables stop
### 配置ssh免密登录
> `生成密钥对`ssh-keygen -t rsa` 三个回车` <br/>
`导入公钥` cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys <br/> 
`拷贝公钥到另外两台服务器上，并导入公钥` scp ~/.ssh/id_rsa.pub root@192.168.1.5:/

## 配置hosts
> vi /etc/hosts
>> 192.168.1.13 hadoop0<br>
192.168.1.5 hadoop1<br>
192.168.1.6 hadoop2<br>


## 安装jdk
> tar zxvf jdk1.6.0_xx.tar.gz -C /usr/local/</br>
 ln -s /usr/local/jdk1.6.0_xx /usr/local/jdk</br>
 vi /etc/profile</br>
>> export JAVA_HOME=/usr/local/jdk  
export PATH=$JAVA_HOME/bin:$PATH  

> `生效配置`source /etc/profile

## 安装hadoop
将在hadoop0作为master，充当namenode,secondarynamenode,jobtracker</br>
hadoop1和hadoop2作为salves,充当datanode,tasktracker</br>
<b>PS:</b>对于配置文件core-site.xml和mapred-site.xml在所有节点中都是相同的内容</br>
所以可以在hadoop0上解压、配置好hadoop后，通过scp拷贝到其它两台机器上,再修改hadoop0上的salves文件加上hadoop1、hadoop2即可</br>
具体步骤如下:</br>
### 解压
> tar -zxvf hadoop-1.1.2.tar.gz -C /opt/</br>
### 修改hadoop的配置文件
> core-site.xml
>> &lt;configuration&gt;</br>
&lt;property&gt;</br>
        &lt;name&gt;fs.default.name&lt;/name&gt;</br>
        &lt;value&gt;hdfs://hadoop0:9000&lt;/value&gt;</br>
        &lt;description&gt;change your own hostname&lt;/description&gt;</br>
    &lt;/property&gt;</br>
    &lt;property&gt;</br>
        &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;</br>
        &lt;value&gt;/opt/hadoop-1.1.2/tmp&lt;/value&gt;</br>
    &lt;/property&gt;</br>
&lt;/configuration&gt;</br>
  
> hdfs-site.xml
>> &lt;configuration></br>
&lt;property></br>
        &lt;name>dfs.replication&lt;/name&gt;</br>
        &lt;value&gt;2&lt;/value&gt;</br>
    &lt;/property&gt;</br>
    &lt;property&gt;</br>
        &lt;name&gt;dfs.permissions&lt;/name&gt;</br>
        &lt;value&gt;false&lt;/value&gt;</br>
    &lt;/property&gt;</br>
&lt;/configuration&gt;</br>
  
> mapred-site.xml
>> &lt;configuration&gt;</br>
&lt;property&gt;</br>
        &lt;name&gt;mapred.job.tracker&lt;/name&gt;</br>
        &lt;value&gt;hadoop0:9001&lt;/value&gt;</br>
        &lt;description&gt;change your own hostname&lt;/description&gt;</br>
    &lt;/property&gt;</br>
&lt;/configuration&gt;</br>
  
> hadoop-env.sh
>> export JAVA_HOME=/usr/local/jdk
  
### 在hadoop1和hadoop2上安装hadoop
在hadoop0上执行下面命令将hadoop拷贝到服务器上
> scp -r /opt/hadoop-1.1.2/ root@hadoop1:/opt/</br>
  scp -r /opt/hadoop-1.1.2/ root@hadoop2:/opt/
  
  
### 在hadoop0上修改hadoop-1.1.2/conf/salves
添加上
> hadoop1</br>
hadoop2</br>
 
## 格式化
在hadoop0节点执行hadoop namenode -format

## 启动
在hadoop0节点执行start-all.sh
</br>

## 验证
* 用jps看进程
* 通过浏览器访问hadoop0:50070及hadoop0:50030查看相应状态

 
