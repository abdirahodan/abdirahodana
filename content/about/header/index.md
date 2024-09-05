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
  .image-container {
    text-align: center; /* Centers the image */
  }

  img.two {
    height: 80%;
    width: 80%;
    border-radius: 50%;
    max-width: 300px; /* Limits image width */
    display: block; /* Ensures the image is a block element */
    margin: auto; /* Centers the image horizontally */
  }

  .text-container {
    padding: 20px; /* Adds spacing around text */
    text-align: center; /* Centers the text */
  }

  .text-container strong {
    display: block; /* Makes the headers block elements */
    margin-bottom: 10px; /* Adds space below the headers */
  }

  .text-container p {
    margin-top: 0;  /* Removes space at the top of paragraphs */
    margin-bottom: 20px; /* Adds space below each paragraph */
  }

</style>
</head>
<body>

<div class="image-container">
  <img class="two" src="/img/0.53.png" alt="drawing"/>
</div>

<div class="text-container">
  <strong>About Me:</strong>
  <p>Long bio</p>
  <strong>Interests:</strong>
  <p>Short bio</p>
</div>

</body>
</html>

