---
## Configure page content in wide column
title: "About Hodan Abdirahman" # leave blank to exclude
number_categories: 0 # set to zero to exclude
---
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>About Page</title>
<style>
  .container {
    display: flex; /* Enables Flexbox */
    align-items: center; /* Centers items vertically */
    justify-content: center; /* Centers items horizontally */
    height: 100vh; /* Full height of the viewport */
  }

  .image-container {
    flex: 1; /* Takes up 1 part of the container */
    text-align: center; /* Centers the image inside the container */
  }

  img.two {
    height: 80%;
    width: 80%;
    border-radius: 50%;
    max-width: 300px; /* Limits image width */
  }

  .text-container {
    flex: 2; /* Takes up 2 parts of the container */
    padding: 20px; /* Adds spacing around text */
  }
</style>
</head>
<body>

<div class="container">
  <div class="image-container">
    <img class="two" src="/img/me.png" alt="drawing"/>
  </div>
  <div class="text-container">
    <strong>About Me:</strong>
    <p>long bio</p>
    <strong>Interests</strong>
    <p>short bio</p>
  </div>
</div>

</body>
</html>


