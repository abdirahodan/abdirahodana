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
    display: flex; /* Enables Flexbox for side-by-side layout */
    align-items: center; /* Aligns items vertically in the center */
    justify-content: center; /* Centers items horizontally */
    height: 100vh; /* Sets the height of the viewport to fill the screen */
  }

  img.two {
    height: 80%;
    width: 80%;
    border-radius: 50%; /* Makes the image round */
    max-width: 300px; /* Restricts the maximum width to prevent oversized images */
    display: block; /* Ensures the image is a block element */
    margin-right: 20px; /* Adds space between the image and the text */
  }

  .text-container {
    max-width: 60%; /* Restricts text width for better readability */
    text-align: left; /* Aligns the text to the left */
  }
  
  /* Styling Markdown-like strong and paragraphs */
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
