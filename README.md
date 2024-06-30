# PHP codes 
<hr>
<br>
<br>

## insert code 

``` php 
<?php
require('database.php');

if (isset(($_POST['submit']))) {
    $firstname = $_POST['firstname'];
    $lastname = $_POST['lastname'];
    $password = $_POST['pass'];
    $email = $_POST['email'];
    $phone = $_POST['phone'];
    $gender  = $_POST['gender'];
    $address = $_POST['address'];
    $hobbies = isset($_POST['hobbies']) ? implode(", ", $_POST['hobbies']) : "";


    $error = [];

    if (empty($firstname)) {
        $errors[] = 'First name is required.';
    }

    if (empty($lastname)) {
        $errors[] = 'Last name is required.';
    }

    if (empty($password)) {
        $errors[] = 'Password is required.';
    } elseif (strlen($password) < 6) {
        $errors[] = 'Password must be at least 6 characters long.';
    }

    if (empty($email)) {
        $errors[] = 'Email is required.';
    } elseif (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = 'Invalid email format.';
    }

    if (empty($phone)) {
        $errors[] = 'Phone number is required.';
    } elseif (!preg_match('/^\+?\d{10,15}$/', $phone)) {
        $errors[] = 'Invalid phone number format.';
    }

    if (empty($gender)) {
        $errors[] = 'Gender is required.';
    }


    if (empty($hobbies)) {
        $error[] = 'hobbies requried atleast 1';
    }


    if (empty($errors)) {
        // Hash the password before storing it
        $hashed_password = password_hash($password, PASSWORD_BCRYPT);

        $sql = "INSERT INTO form (firstname, lastname, pass, email, phone, gender, address,hobbies)
                VALUES ('$firstname', '$lastname', '$hashed_password', '$email', '$phone', '$gender', '$address','$hobbies')";

        if (mysqli_query($conn, $sql)) {
            echo "Your data has been added to the forms database.";
        } else {
            echo "Error: " . $sql . "<br>" . mysqli_error($conn);
        }
    } else {
        foreach ($errors as $error) {
            echo $error . "<br>";
        }
    }
}
```

<hr />
<br >
<br>


## update 

``` php
<?php
require('database.php');
if (isset($_POST['update'])) {
    $id = $_POST['id'];
    $fname = $_POST['firstname'];
    $lastname = $_POST['lastname'];
    $password = $_POST['pass'];
    $email = $_POST['email'];
    $phone = $_POST['phone'];
    $gender  = $_POST['gender'];
    $adress = $_POST['address'];
    $hobbies = isset($_POST['hobbies']) ? implode(", ", $_POST['hobbies']) : "";

    $sql = "UPDATE form 
    SET firstname='$fname', lastname='$lastname', pass='$password', email='$email', phone='$phone', gender='$gender', address='$address'
      hobbies='$hobbies' WHERE id='$id'";

    $result = mysqli_query($conn, $sql);

    if ($result == TRUE) {

        echo "Record updated successfully.";
    } else {

        echo "Error:" . $sql . "<br>" . $conn->error;
    }
}
if (isset($_GET['id'])) {

    $userid = $_GET['id'];
    $sql = "SELECT * FROM `form` WHERE `id`='$userid'";
    $result = mysqli_query($conn, $sql);
    if ($result->num_rows > 0) {
        while ($row = $result->fetch_assoc()) {
            $firstname = $_POST['firstname'];
            $lastname = $row['lastname'];
            $password = $row['pass'];
            $email = $row['email'];
            $phone = $row['phone'];
            $gender  = $row['gender'];
            $adress = $row['address'];
            $hobbies = isset($_POST['hobbies']) ? implode(", ", $_POST['hobbies']) : "";
        }
    }
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        form {
            background-color: #ffffff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input[type="text"],
        input[type="password"],
        input[type="email"],
        input[type="phone"],
        select,
        textarea {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }

        input[type="submit"]:hover {
            background-color: #45a049;
        }

        textarea {
            resize: vertical;
        }
    </style>
</head>

<body>
    <form action="insert.php" method="post">
        <label for="firstname">first Name</label>
        <input type="text" name="firstname" id="firstname" value="<?php echo $fname ?>">
        <label for="firstname">last Name</label>
        <input type="text" name="lastname" id="lastname" value="<?php echo $lastname ?>">
        <label for="pass">Password</label>
        <input type="password" name="pass" id="pass" value="<?php echo $password ?>">
        <label for="email">Email</label>
        <input type="email" name="email" id="email" value="<?php echo $email ?>">
        <label for="phone">Phone</label>
        <input type="phone" name="phone" id="phone" value="<?php echo $phone ?>">
        <label for="gender">Gender</label>
        <select name="gender" id="gender" value="<?php echo $gender ?>">
            <option value="">Select</option>
            <option value="male" name="male">Male</option>
            <option value="female">Female</option>
        </select>



        <label for="hobbies">Hobbies:</label>
        <input type="checkbox" id="sports" name="hobbies[]" value="sports" value="<?php echo $hobbies ?>">
        <label for="sports">Sports</label>
        <input type="checkbox" id="music" name="hobbies[]" value="music" value="<?php echo $hobbies ?>">
        <label for="music">Music</label>
        <input type="checkbox" id="reading" name="hobbies[]" value="reading" value="<?php echo $hobbies ?>">
        <label for="reading">Reading</label><br><br>



        <label for="address">Address</label>
        <input type="text" name="address" id="address" value="<?php echo $address ?>">
        <input type="submit" name="submit" id="submit">
    </form>

</body>

</html>
```

<br>
<hr>
<br>

## Delete

```php
<?php
require('database.php');

if (isset($_GET['id'])) {
    $userid = $_GET['id'];


    $sql = "DELETE FROM form WHERE id='$userid';";
    $result = mysqli_query($conn, $sql);

    if ($result == TRUE) {

        echo "Record deleted successfully.";
    } else {

        echo "Error:" . $sql . "<br>" . $conn->error;
    }
}

```

<br>
<hr>
<hr>


## show 

``` php
<?php
require('database.php');
$sql = "SELECT * FROM form";

$result = mysqli_query($conn, $sql);

?>



<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/css/bootstrap.min.css">
    !<table class="table">
        <thead>
            <tr>
                <th>id</th>
                <th>firstname</th>
                <th>lastname</th>
                <th>pass</th>
                <th>email</th>
                <th>phone</th>
                <th>gender</th>
                <th>address</th>
            </tr>
        </thead>
        <tbody><?php

                if ($result->num_rows > 0) {
                    while ($row = $result->fetch_assoc()) {
                ?>
                    <tr>
                        <td><?php echo $row['id'] ?></td>
                        <td><?php echo $row['firstname'] ?></td>
                        <td><?php echo $row['lastname'] ?></td>
                        <td><?php echo $row['pass'] ?></td>
                        <td><?php echo $row['email'] ?></td>
                        <td><?php echo $row['phone'] ?></td>
                        <td><?php echo $row['gender'] ?></td>
                        <td><?php echo $row['address'] ?></td>
                        <td><a class="btn btn-info" href="update.php?id=<?php echo $row['id']; ?>">Edit</a>&nbsp;<a class="btn btn-danger" href="delete.php?id=<?php echo $row['id']; ?>">Delete</a></td>
                    </tr>
            <?php }
                }
            ?>
        </tbody>
    </table>






</body>

</html>
```

# Note crud done 


## DB code 

``` php 
<?php

$conn = mysqli_connect("localhost", "root", "", "regifrom");

if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
echo "Connected successfully";

```

<hr>
<br>
<br>

## form 

``` php 
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        form {
            background-color: #ffffff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input[type="text"],
        input[type="password"],
        input[type="email"],
        input[type="phone"],
        select,
        textarea {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }

        input[type="submit"]:hover {
            background-color: #45a049;
        }

        textarea {
            resize: vertical;
        }
    </style>
</head>

<body>
    <form action="insert.php" method="post">
        <label for="firstname">first Name</label>
        <input type="text" name="firstname" id="firstname">
        <label for="firstname">last Name</label>
        <input type="text" name="lastname" id="lastname">
        <label for="pass">Password</label>
        <input type="password" name="pass" id="pass">
        <label for="email">Email</label>
        <input type="email" name="email" id="email">
        <label for="phone">Phone</label>
        <input type="phone" name="phone" id="phone">
        <label for="gender">Gender</label>
        <select name="gender" id="gender">
            <option value="">Select</option>
            <option value="male" name="male">Male</option>
            <option value="female">Female</option>
        </select>



        <label for="hobbies">Hobbies:</label>
        <input type="checkbox" id="sports" name="hobbies[]" value="sports">
        <label for="sports">Sports</label>
        <input type="checkbox" id="music" name="hobbies[]" value="music">
        <label for="music">Music</label>
        <input type="checkbox" id="reading" name="hobbies[]" value="reading">
        <label for="reading">Reading</label><br><br>



        <label for="address">Address</label>
        <input type="text" name="address" id="address">
        <input type="submit" name="submit" id="submit">
    </form>

</body>

</html>
```

<hr>
<br>
<br>

## prac 

``` php 

<h1> <b>sum of digit program </b> </h1>

<?php

$num = 1235;
$sum = 0;
$rem = 0;

for ($i = 0; $i < strlen($num); $i++) {

    $rem = $num % 10;
    $sum = $sum + $rem;
    $num = $num / 10;
}

echo "sum of digits is $sum";

?>
<br>
<h1> <b>fact program </b> </h1>
<?php


$num = 3;
$fact = 1;


for ($i = $num; $i >= 1; $i--) {

    $fact = $fact * $i;
}
echo " your fact is $fact";
?>
<br>

<?php
$string = "php";

$rev = strrev($string);

if ($rev == $string) {
    echo "it can be same when we revre s it ";
} else {
    echo "its not palindrome";
}
?>


<?php

$a = 22;
$b = 33;
$third = 0;

$third = $a;
$a = $b;
$b = $third;

echo "After swapping:<br><br>";
echo "a =" . $a . "  b=" . $b;

?>

<?php
$a = 999;
$b = 888;
$a = $a + $b;
$b = $a - $b;
$a = $a - $b;

echo "After swapping:<br><br>";
echo "a =" . $a . "  b=" . $b;

?>

<?php

$number = range('a', 'z');


for ($i = 0; $i < 10; $i++) {
    for ($j = 10; $j >= $i; $j--) {
        echo $number[$i];
    }
    echo "<br>";
}

?>

<?php

for ($i = 1; $i <= 5; $i++) {
    for ($j = 1; $j <= 5; $j++) {
        echo "*";
    }
    echo "<br>";
}




?>
```
