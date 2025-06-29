#index.html

<!DOCTYPE html>
<html>
<head>
    <title>Pollen Classification</title>
</head>
<body>
    <h1>Upload Pollen Image</h1>
    <form method="post" action="/predict" enctype="multipart/form-data">
        <input type="file" name="file" accept="image/*">
        <input type="submit" value="Predict">
    </form>
    <a href="/logout">Logout</a>
</body>
</html>

#loguot.html

<!DOCTYPE html>
<html>
<head>
    <title>Logout</title>
</head>
<body>
    <h1>You have been logged out</h1>
    <a href="/">Return to Home</a>
</body>
</html>


prediction.html

<!DOCTYPE html>
<html>
<head>
    <title>Prediction Result</title>
</head>
<body>
    <h1>Prediction Result</h1>
    <p>Predicted Pollen Class: {{ prediction }}</p>
    <a href="/">Upload Another</a>
</body>
</html>
