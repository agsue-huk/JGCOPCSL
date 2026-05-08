<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Subway Runner Multiplayer</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }
    body {
      background: linear-gradient(to top, #1e3c72, #2a5298);
      overflow: hidden;
      height: 100vh;
      position: relative;
    }
    #track {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 100%;
      background: repeating-linear-gradient(
        90deg,
        #222, #222 50px,
        #333 50px, #333 100px
      );
      background-size: 100px 100%;
    }
    .player {
      position: absolute;
      width: 50px;
      height: 50px;
      background-size: cover;
    }
    #player1 {
      background: url('https://via.placeholder.com/50/FFC0CB/FF0066?text=D1') no-repeat center center;
      left: 100px;
      bottom: 50px;
    }
    #player2 {
      background: url('https://via.place
