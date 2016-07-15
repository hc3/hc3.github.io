---
layout: post
title:  "Criando jogo Batalha Naval"
date:   2016-07-14 10:56:00
categories: Javascript básico.
---

Falaa galera, nesse post vamos criar um jogo de batalha naval baseado no capitulo 08 do livro Head First Javascript Prograaming, vamos abordar conceitos práticos de estrutura de controle e repetição, objetos, MVC, CSS e HTML, a parte de CSS e HTML é não será explicada o melhor é vc já entender pelo menos o básico das duas coisas antes mesmo de ir pra o javascript, o código fonte completo está no meu [github](https://github.com/hc3/BattleShip), lá vocês podem fazer o download das imagens que serão usadas no jogo, então sem mais vamos ao código.

<h3> Iniciando e trabalhando conceitos </h3>
o jogo batalha naval é bastante conhecido, vamos ter uma grade onde os navios vão estar escondidos e essa grade é composta por linhas e colunas, sendo que as linhas são referenciadas por letras e as colunas por números, vamos agora ao design do jogo lembrando que design ( HTML e CSS ), já estamos cansados de saber que o html será responsável pela estrutra da página e o css vai aplicar estilo a mesma e o javascript vai aplicar o comportamento ( behavior ).
<h5>1º PASSO</h5>
````html
  <!doctype html>

  <html>
  	<head>
  		<meta charset="UTF-8">
  		<title>Batalha naval</title>
  		<style>
  			body {
  				background-color: black;
  			}
  			div#board {
  				position : relative;
  				width: 1024px;
  				height: 863px;
  				margin: auto;
  				background: url("board.jpg") no-repeat;
  			}
  		</style>
  	</head>
  	<body>
  		<div id="board">

  		</div>
  		<script src="jogo.js"></script>
  	</body>
  </html>
````
podemos ver aqui a criação de uma div que recebe o id="board" com esse id podemos apliar estilo usando css como podemos ver em <b>div#board</b>, body recebe uma cor de fundo a div board vai tem uma posição relativa tem um width e heigth, uma magem automática e um background que é a justamente a imagem onde o jogo vai rodar em cima o no-repeat é para que a imagem seja exibida apenas uma vez, aqui já podemos dar um f5 e ver a página atualizando.

<h5>2º PASSO</h5>

````html
<table>
<tr>
  <td id="00"></td>
  <td id="01"></td>
  <td id="02"></td>
  <td id="03"></td>
  <td id="04"></td>
  <td id="05"></td>
  <td id="06"></td>
</tr>
<tr>
  <td id="10"></td>
  <td id="11"></td>
  <td id="12"></td>
  <td id="13"></td>
  <td id="14"></td>
  <td id="15"></td>
  <td id="16"></td>
</tr>
<tr>
  <td id="20"></td>
  <td id="21"></td>
  <td id="22"></td>
  <td id="23"></td>
  <td id="24"></td>
  <td id="25"></td>
  <td id="26"></td>
</tr>
<tr>
  <td id="30"></td>
  <td id="31"></td>
  <td id="32"></td>
  <td id="33"></td>
  <td id="34"></td>
  <td id="35"></td>
  <td id="36"></td>
</tr>
<tr>
  <td id="40"></td>
  <td id="41"></td>
  <td id="42"></td>
  <td id="43"></td>
  <td id="44"></td>
  <td id="45"></td>
  <td id="46"></td>
</tr>
<tr>
  <td id="50"></td>
  <td id="51"></td>
  <td id="52"></td>
  <td id="53"></td>
  <td id="54"></td>
  <td id="55"></td>
  <td id="56"></td>
</tr>
<tr>
  <td id="60"></td>
  <td id="61"></td>
  <td id="62"></td>
  <td id="63"></td>
  <td id="64"></td>
  <td id="65"></td>
  <td id="66"></td>
</tr>
</table>
````
o próximo passo é a criação de uma tabela vamos entender bem o que está sendo feito essa tabela possui 7 <tr> e cada tr possui 7 <td> isso é exatamente o que a imagem board.jpg tem uma tabela 7x7 e com isso podemos criar as células onde os jogadores poderam tentar acertar nos navios podemos notar aqui que essas células no jogo de batalha naval tem uma nomenclatura diferente lá a célula "00" é "A1".

<h5>3º PASSO</h5>

````html
<div id="board">
<div id="messageArea"></div>
  <table>
  ...
  </table>
  <form>
    <input type="text" id="guessInput" placeholder="A0">
    <input type="button" id="fireButton" value="Fire!">
  </form>
</div>
````
criamos uma área para interação com usuário primeiro foi criada uma div com o id="messageArea" onde vão ser exibidas as mensagens ao usuário e depois criamos um form para o usuário fazer sua jogada.
<br/>

vamos aplicar estilo, se agora mesmo dermos um F5 para atualizar a página podemos ver que está tudo desconfigurado e fora do lugar, vamos aplicar um estilo para que tudo fique no seu devido lugar, como disse no inicio do post não irei me aprofundar em html e css mas ai vai uma breve explicação usamos a posição absoluta para poder colocar os elementos nos locais exatos a área de mensagem deve ficar no canto superior direito e o input deve ficar no canto inferior esquerdo, criamos também uma tabela com o tamanho exato do desenho em board.jpg e o <td> são as células que vão ter exatos 94x94 px, foi criado também as duas classes .hit e .miss que vão ser usadas futuramente com isso terminamos toda a parte de estilo e desenhos.

````css
div#messageArea {
  position: absolute;
  top: 0px;
  left: 0px;
  color: rgb(83, 175, 19);
}
table {
  position: absolute;
  left: 173px;
  top: 98px;
  border-spacing: 0px;
}
td {
  width: 94px;
  height: 94px;
}
form {
  position: absolute;
  bottom: 0px;
  right: 0px;
  padding: 15px;
  background-color: rgb(83, 175, 19);
}
form input {
  background-color: rgb(152, 207, 113);
  border-color: rgb(83, 175, 19);
  font-size: 1em;
}
.hit {
background: url("ship.png") no-repeat center center;
}
.miss {
background: url("miss.png") no-repeat center center;
}
````
podemos testar as classes .hit e .miss fazendo :

````html
<tr>
  <td id="10"></td> <td id="11"></td> <td id="12"></td> <td id="13" class="hit"></td>
  <td id="14"></td> <td id="15"></td> <td id="16"></td>
</tr>
````
veremos que na célula B3 vamos ter um navio que representa um hit ou seja quando fizermos uma jogada que for certa é essa classe que vamos colocar no html.
<br/>

<h3> Implementando o design do jogo! </h3>

já criamos todo o layout do jogo com html e css agora vamos implementar os comportamentos ( BEHAVIOOOR ) e agora é só JS da aqui a frente, rs, primeiro conceito que quero passar aqui é de responsabilidade única ou MVC ( model view controller ), acredito que hoje em dia essa sigla seja bastante conhecida já que a galera ve isso em muitos frameworks de PHP , Java ... e tarará, primeiro de tudo vamos criar um arquivo chamado jogo.js colocar na mesma pasta do html e css abrindo arquivo vamos começar criando a view.
<h5> VIEW </h5>
a view vai ter os métodos para exibir mensagens dependendo do que estiver acontecendo no jogo, por exemplo se o usuário acertar uma jogada vai exibir "HIT" se não "MISS" vamos ver a implementação desse objeto.

````js
var view = {
  displayMessage: function(msg){
    var messageArea = document.getElementById("messageArea");
    messageArea.innerHTML = msg;
  },
  displayHit: function(location){
    var cell = document.getElementById(location);
    cell.setAttribute("class","hit");
  },
  displayMiss: function(location){
    var cell = document.getElementById(location);
    cell.setAttribute("class","miss");
  }
};
````
displayMessage = podemos ver que displayMessage captura o elemento usando getElementById passando o ID do elemento a partir da ai conseguimos manipular o DOM e usando o innerHTML colocamos a mensagem na <div id="messageArea">.
displayHit e displayMiss =  primeiro vamos pegar o location que é o ID do <td> ou seja a célula que vai ser atacada usando o getElementById podemos pegar o elemento e depois usando o método setAttribute passamos duas strings como argumento a primeira é o tipo que será class a segunda é a qual será o valor dessa classe e podemos fazer isso tanto para o hit quanto para o miss, feito isso podemos realizar um teste básico:

````js
view.displayMiss("00");
view.displayHit("34");
view.displayMiss("55");
view.displayHit("12");
view.displayMiss("25");
view.displayHit("26");
view.displayMessage("Tap tap, is this thing on?");
````

<h5> o Model </h5>
