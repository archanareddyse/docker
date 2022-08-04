1.Create the network.
 $ docker network create todo-app
 2.Start a MySQL container and attach it to the network. We’re also going to define a few environment variables that the database will use to initialize the database (see the “Environment Variables” section in the MySQL Docker Hub listing).
  PS> docker run -d `
     --network todo-app --network-alias mysql `
     -v todo-mysql-data:/var/lib/mysql `
     -e MYSQL_ROOT_PASSWORD=secret `
     -e MYSQL_DATABASE=todos `
     mysql:5.7
3. To confirm we have the database up and running, connect to the database and verify it connects.
 docker exec -it <mysql-container-id> mysql -u root -p
 mysql>SHOW DATABASES;
 mysql>exit
Run your app with MySQL🔗
1.Note: for MySQL versions 8.0 and higher, make sure to include the following commands in mysql.
 $ ALTER USER 'root' IDENTIFIED WITH mysql_native_password BY 'secret';
 $ flush privileges;
 PS> docker run -dp 3000:3000 `
   -w /app -v "$(pwd):/app" `
   --network todo-app `
   -e MYSQL_HOST=mysql `
   -e MYSQL_USER=root `
   -e MYSQL_PASSWORD=secret `
   -e MYSQL_DB=todos `
   node:12-alpine `
   sh -c "yarn install && yarn run dev"
4.Open the app in your browser and add a few items to your todo list.
5.Connect to the mysql database and prove that the items are being written to the database. Remember, the password is secret.
$ docker exec -it <mysql-container-id> mysql -p todos
And in the mysql shell, run the following:
mysql> select * from todo_items;
