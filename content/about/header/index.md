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
  body, html {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .container {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 80%;
  }

  img.two {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    display: block;
    margin-right: 20px;
    object-fit: cover;
  }

  .text-container {
    max-width: 60%;
    text-align: left;
  }
  
  strong {
    display: block;
    font-size: 1.5em;
    margin-bottom: 0.5em;
  }

  p {
    margin-top: 0;
    margin-bottom: 1em;
  }
</style>
</head>
<body>

<div class="container">
  <img class="two" src="/img/me.png" alt="Portrait of me"/>
  <div class="text-container">
    <strong>About Me:</strong>
    <p>Long bio</p>
    <
