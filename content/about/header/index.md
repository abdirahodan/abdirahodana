---
## Configure page content in wide column
title: "About Hodan Abdirahman" # leave blank to exclude
number_categories: 0 # set to zero to exclude
---
img.two {
  width: 200px; /* Set both width and height to the same value */
  height: 200px; /* This will ensure the image is square */
  border-radius: 50%; /* This will make the square image appear as a circle */
  display: block; /* Ensures the image is a block element */
  margin-right: 20px; /* Adds space between the image and the text */
  object-fit: cover; /* Ensures the image covers the area without distorting */
}

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>About Page</title>
<style>
  .container {
    display: flex; /* Enables Flexbox for side-by-side layout */
    align-items: center; /* Aligns items vertically in the center */
    justify-content: center; /* Centers items horizontally */
    height: 100vh; /* Sets the height of the viewport to fill the screen */
  }

  img.two {
    width: 200px; /* Ensures the image is a square */
    height: 200px; /* Ensures the image is a square */
    border-radius: 50%; /* Makes the image round */
    display: block; /* Ensures the image is a block element */
    margin-right: 20px; /* Adds space between the image and the text */
    object-fit: cover; /* Ensures the image covers the square area without distorting */
  }

  .text-container {
    max-width: 60%; /* Restricts text width for better readability */
    text-align: left; /* Aligns the text to the left */
  }
  
  strong {
    display: block; /* Treats the header-like elements as block for better control */
    font-size: 1.5em; /* Larger text for headers */
    margin-bottom: 0.5em; /* Space below headers */
  }

  p {
    margin-top: 0; /* Removes top margin from paragraphs */
    margin-bottom: 1em; /* Adds bottom margin for spacing paragraphs */
  }

</style>
</head>
<body>

<div class="container">
  <img class="two" src="/img/me.png" alt="Portrait of me"/>

  <div class="text-container">
    <strong>About Me:</strong>
    <p>Long bio</p>
    <strong>Interests:</strong>
    <p>Short bio</p>
  </div>
</div>

</body>
</html>
