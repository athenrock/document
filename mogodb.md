
## Ubuntu 安装MongoDB

    http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

开始MongoDB

    sudo service mongod start


## mogodb 备份
	mongodump -d ykt -o Documents
-d指定需要备份的数据库，-o指定备份位置，上述表示备份ykt数据库到与当前mongodump命
令同一位置Documents目录下


## mongodb 恢复并重命名为ykt2

	mongorestore -d ytk2 --drop Documents/ykt