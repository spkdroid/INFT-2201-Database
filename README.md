<h1 align="center" style="text-align: center; padding-bottom: 20px;">
  <br>
 <img src="https://github.com/spkdroid/INFT-2201-Database/blob/master/logo.png" alt="Bike Index" width="220"/>
  <br>
  INFT-2201-Docker Database with CRUD
  <br>
</h1>

## Step 1: Install Docker
Ensure that you have Docker installed on your system. You can download it from [Docker's official website](https://www.docker.com/).

## Step 2: Create a Docker Compose File
Create a `docker-compose.yml` file with the following content:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:latest
    container_name: mysql_container
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: test_db
      MYSQL_USER: user
      MYSQL_PASSWORD: userpassword
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

## Step 3: Start the MySQL Container
Run the following command in the same directory as your `docker-compose.yml` file:

```sh
docker-compose up -d
```

This will spin up a MySQL container running in the background.

## Step 4: Verify MySQL is Running
Run the following command to check running containers:

```sh
docker ps
```

You should see `mysql_container` running.

## Step 5: Connect to MySQL Inside the Container
You can connect to MySQL inside the container using the following command:

```sh
docker exec -it mysql_container mysql -u user -p
```

Enter the password `userpassword` when prompted.

## Step 6: Write a Simple PHP Program to Interact with the MySQL Container
Create a `db_connect.php` file with the following content:

```php
<?php
$host = "localhost";
$dbname = "test_db";
$username = "user";
$password = "userpassword";

try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbname;charset=utf8", $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "Connected successfully";
} catch (PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
?>
```

## Step 7: Run the PHP Program
Ensure you have PHP installed and run the script:

```sh
php db_connect.php
```

If everything is set up correctly, you should see `Connected successfully` printed on the screen.

## Step 8: CRUD Operations in PHP

## Create (Insert Data)
```php
<?php
$sql = "INSERT INTO users (name, email) VALUES (:name, :email)";
$stmt = $pdo->prepare($sql);
$stmt->execute(['name' => 'John Doe', 'email' => 'john@example.com']);
echo "User added successfully";
?>
```

## Read (Fetch Data)
```php
<?php
$sql = "SELECT * FROM users";
$stmt = $pdo->query($sql);
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
    echo "<p>User: " . htmlspecialchars($row['name']) . " - Email: " . htmlspecialchars($row['email']) . "</p>";
}
?>
```

## Update (Modify Data)
```php
<?php
$sql = "UPDATE users SET email = :email WHERE name = :name";
$stmt = $pdo->prepare($sql);
$stmt->execute(['email' => 'newemail@example.com', 'name' => 'John Doe']);
echo "User updated successfully";
?>
```

## Delete (Remove Data)
```php
<?php
$sql = "DELETE FROM users WHERE name = :name";
$stmt = $pdo->prepare($sql);
$stmt->execute(['name' => 'John Doe']);
echo "User deleted successfully";
?>
```
