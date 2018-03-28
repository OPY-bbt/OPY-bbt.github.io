<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>hairline</title>
  <style>
    .base {
      width: 150px;
      height: 10px;
      color: purple;
      margin-top: 10px;
      margin-bottom: 20px;
    }
    .hairLine1 {
      position: relative;
    }
    .hairLine1::after {
      content: '';
      height: 1px;
      border-bottom: 1px solid black;
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      transform: scaleY(0.5);
      transform-origin:  0 100%;
    }

    .hairLine2 {
      background: linear-gradient(to bottom , transparent 50%, black 50%, black 50%) left bottom repeat-x;
      background-size: 100% 1px;
    }

    /* chrome */
    .hairLine3 {
      box-shadow: inset 0px -0.5px 0px 0px black;
    }

    /* safari */
    .hairLine4 {
      border-bottom: 0.5px solid black;
    }

  </style>
</head>
<body>
  <div class="base hairLine1">all</div>
  <div class="base hairLine2">all</div>
  <div class="base hairLine3">chrome</div>
  <div class="base hairLine4">safari</div>
</body>
</html>