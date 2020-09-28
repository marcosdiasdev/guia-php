---
tags: [Didática]
title: 'PHP: um guia rápido para quem veio de JavaScript'
created: '2019-09-12T14:12:46.874Z'
modified: '2020-09-28T14:39:16.597Z'
---

# PHP: um guia rápido para quem veio de JavaScript

## 1 Olá, mundo!

Após ter instalado os programas necessários e configurado o seu ambiente de desenvolvimento, crie um arquivo `index.php` e insira o seguinte conteúdo:

```php
<?php
  echo "Olá, mundo!";
?>
```

Se sua instalação do PHP estiver disponível nas variáveis de ambiente, abra um terminal, acesse a pasta onde salvou o arquivo e execute o seguinte comando:

```sh
php -S localhost:8080
```

Este comando irá fazer com que o servidor embutido do PHP disponibilize o conteúdo do diretório atual no endereço local na porta 8080 por meio do protocolo HTTP. Para visualizar o resultado, abra seu navegador e acesse [http://localhost:8080](http://localhost:8080).

 Se o sistema operacional indicar que o comando `php` é desconhecido, certamente sua instalação do PHP não está registrada na variável de ambiente PATH. Se estiver utilizando Windows, veja como resolver isso a partir dos 4 minutos [deste vídeo](https://www.youtube.com/watch?time_continue=240&v=KwEilZK5d04&feature=emb_logo).

## 2 Variáveis e tipos

Toda variável deve ser referenciada com um cifrão (`$`) em PHP. Além disso, não há um comando específico para declaração de variáveis, basta inicializar e começar a usar:

```php
<?php
  $criador = "Rasmus Lerdof";
?>
```

Além disso, a tipagem das variáveis é fraca e dinâmica, o que quer dizer que é possível realizar operações entre variáveis de tipos diferentes e também modificar o tipo de valor que uma variável armazena:

```php
<?php
  $a = "2";
  $b = 2;

  echo $a + $b; // 4 (em JavaScript seria "22")

  $a = 3;
  echo $a + $b; // 5
?>
```

## 3 Estruturas de controle e foreach

A linguagem PHP possui as mesmas estruturas de controle básicas existentes na linguagem JavaScript: `if/else`, `switch/case`, `while`, `do/while` e `for`:

```php
<?php
  for($i = 1; $i < 100; $i++) {
    if($i % 3 == 0 && $i % 2 == 0) {
      echo $i;
    }
  }
?>
```

Além dessas, há uma estrutura de repetição muito utilizada para percorrer arrays, a `foreach()`, que nos lembra a `for/of` do JavaScript:

```php
<?php
  $letras = ['a', 'b', 'c', 'd', 'e', 'f', 'g'];
  foreach($letras as $letra) {
    echo "$letra<br>";
  }
?>
```

## 4 Arrays

Para quem vem JavaScript, o comportamento dos arrays em PHP é praticamente o mesmo, exceto pelo fato de que eles não são objetos. Logo, não teremos acesso a métodos como `Array.prototype.push()` ou `Array.prototype.pop()`. No entanto, existem as funções `array_push` e `array_pop`, além de outras, com comportamentos equivalentes.

```php
<?php
  $paises = ["Argentina", "Brasil", "Chile", "Dinamarca", "Equador"];
  echo $paises[4]; // Equador

  array_pop($paises); // Equador será removido
  array_push($paises, "Eslovênia");
  echo $paises[4]; // Eslovênia
?>
```

Como não temos acesso a uma propriedade `length`, utilizaremos a função `count()` (ou sua equivalente, `sizeof()`) para obter o tamanho de um array:

```php
<?php
  $paises = ["Argentina", "Brasil", "Chile"]
  echo count($paises); // 3
?>
```

### Arrays associativos

Arrays associativos são um tipo específico de array com posições nomeadas ao invés de apenas identificadas por um índice numérico.

```php
<?php
  $bandeiras = [
    "brasil" => "🇧🇷",
    "eua" => "🇺🇸",
    "jamaica" => "🇯🇲"
  ];

  echo $bandeiras["brasil"]; // 🇧🇷
?>
```

Para imprimir um array, array associativo ou objeto sem que seja necessário percorrer cada uma de suas posições, experimente as funções `print_r()` e `var_dump()`:

```php
<?php
  print_r($bandeiras);

  var_dump($bandeiras);
?>
```

Para percorrer um array associativo e obter acesso às chaves (nomes das posições) e valores, utilize o loop `foreach` da seguinte forma:

```php
foreach($bandeiras as $chave => $valor)
{
  echo $chave . ":" . $valor;
}
```

## 5 Strings

Strings não são objetos em PHP, como são em JavaScript. Isso quer dizer que não teremos acesso a métodos e propriedades para manipulação das strings. No entanto, há recursos disponíveis que deverão atender às nossas necessidades.

### Concatenação de strings

Em PHP, é possível concatenar strings com outras strings, variáveis ou expressões, com algumas pequenas diferenças: 

```php
<?php
  $nome = "Bill";
  $saudacao = "Olá, $nome";
  $saudacaoLiteral = 'Olá, $nome';
  $saudacaoConcatenada = "Olá, " . $nome;
  $saudacaoComChaves = "Olá, {$nome}";

  echo $saudacao; // Olá, Bill
  echo $saudacaoLiteral; // Olá, $nome
  echo $saudacaoConcatenada; // Olá, Bill
  echo $saudacaoComChaves; // Olá, Bill
?>
```

No exemplo acima, há três pontos a serem observados:
- O uso de aspas duplas permite que a variável `$nome` seja interpretada e seu conteúdo exibido. 
- Já as aspas simples (aspas literais) fazem com que a string seja impressa caractere por caractere.
- Na variável `$saudacaoConcatenada`, utilizamos o operador de concatenação (`.`) para unir a string à variável `$nome`.
- Chaves (`{}`) podem ser utilizadas para destacar uma variável dentro de uma string que utilize aspas duplas.

### Obtendo o tamanho de uma string

Como não temos acesso a uma propriedade `length`, como em JavaScript, utilizaremos a função `strlen()` para obter o tamanho de uma string:

```php
<?php
  $linguagem = "PHP";
  echo strlen($linguagem); // 3
?>
```

### Quebrando e juntando strings

Lembra do método `split()` disponível nas strings em JavaScript? A função `explode()` será sua substituta no PHP:

```php
<?php
  $linguagens = "c c++ php java javascript ruby python";
  $listaDeLinguagens = explode(" ", $linguagens);

  echo $listaDeLinguagens[2]; // php

  $linguagens = implode(", ", $listaDeLinguagens);
  echo $linguagens; // c, c++, php, java, javascript, ruby, python
?>
```

## 6 Funções e tipos de parâmetro e retorno

Funções em PHP têm a exata mesma sintaxe da linguagem JavaScript:

```php
<?php
  function soma($x, $y) {
    return $x + $y;
  }
?>
```
É possível adicionar tipos aos parâmetros e retorno de uma função. Isso auxiliará quem utilizar a função a identificar que tipos de valores passar e que tipo de valor esperar como retorno. Na função a seguir, especificamos que as variáveis `$x` e `$y` deverão ser do tipo `int` e que o valor retornado pela função também ser do tipo `int`:

```php
<?php
  function somaTipada(int $x, int $y): int {
    return $x + $y;
  }
?>
```

## 7 Objetos

A manipulação de objetos em PHP é bastante semelhante a JavaScript. Atente-se apenas para o fato de que o operador de objeto passa a ser uma seta (`->`) em vez de um ponto:

```php
$agora = new DateTime('NOW');
echo $agora->format('Y-m-d H:i:s');
```


