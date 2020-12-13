# PHP: do básico ao intermediário

## Imprimindo HTML

A principal finalidade da linguagem PHP é gerar conteúdo dinâmico em páginas da web. Isso significa que ela nos possibilitará, por exemplo, construir toda a estrutura de páginas web com base em regras de negócio processadas no servidor, antes mesmo de a página ser recebida pelo navegador do usuário.

Por exemplo, para gerar uma página web que exiba o horário atual, basedo no horário do servidor, podemos fazer o seguinte:

```php
<?php
 echo "<p>Olá. Hoje é " . date('d/m/Y') . ", e são " . date('H:i:s') . "</p>";
?>
```

> O exemplo acima se difere de simplesmente obter a data e o horário utilizando JavaScript no navegador pois não depende do horário do dispositivo do usuário.

Podemos utilizar estruturas de controle junto à impressão de tags HTML:

```php
<?php
  for($i = 1; $i <= 10; $i++) {
    echo "<p>Parágrafo {$i}</p>";
  }
?>
```

E intercalar código HTML e código PHP:

```php
<body>
  
<?php
  $exibir_titulo = true;
  if($exibir_titulo){
?>

  <h1>Cadastro</h1>

<?php
  }
?>

</body>
```

Se preferir, você pode substituir as chaves por um recurso específico da linguagem PHP, que consiste em utilizar `:` na abertura da estrutura e o prefixo `end` acompanhado do nome da estrutura no fechamento:

```php
<body>
  
<?php
  $exibir_titulo = true;
  if($exibir_titulo):
?>

  <h1>Cadastro</h1>

<?php
  endif;
?>

</body>
```

## Retornando JSON

Caso deseje construir uma aplicação que retorne JSON, primeiramente, é importante definir o cabeçalho para que o cliente interprete seu conteúdo corretamente, por meio da função `header()`. Para que o conteúdo a ser impresso seja formatado em JSON, utilize a função `json_decode()`. O exemplo a seguir retornará um objeto JSON com dados de data e hora atuais:

```php
<?php
  header('Content-Type: application/json');

  $agora = new DateTime('NOW');

  echo json_encode($agora);
?>
```

## Incluindo arquivos e reaproveitando código

As funções `include()` e `require()` podem ser utilizadas para realizar a inclusão de um arquivo PHP dentro de outro, o que nos possibilita separar nosso código e reaproveitá-lo em lugares diferentes de uma aplicação.

> A diferença entre essas duas funções é que a `require()` dispara um erro fatal caso não consiga realizar a inclusão do arquivo.

Um dos usos mais comuns dessas funções é para separação e reutilização de código HTML repetido. Podemos, por exemplo, manter separados em arquivos únicos o cabeçalho e o rodapé de nosso site e reaproveitá-lo em páginas diferentes:

> header.php
```php
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Documento</title>
</head>
<body>
```

> footer.php
```php
  <footer><p>Todos os direitos reservados.</p></footer>
</body>
</html>
```

Para aproveitar `header.php` e `footer.php` na construção de uma página, podemos fazer o seguinte:

> index.php
```php
<?php
include("header.php");
echo "<p>Este é o conteúdo da página inicial.</p>";
include("footer.php");
?>
```

### Injetando variáveis em um arquivo incluído

Imagine que no exemplo anterior queiramos poder definir um título único para cada página que realizar a inclusão do `header.php`. Para isso, podemos modificar este arquivo para utilizar uma variável `$titulo` como conteúdo da tag `<title>`:

> header.php
```php
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><?= $titulo ?></title>
</head>
<body>
```

E definir a variável `$titulo` no arquivo `index.php` que realizará a inclusão do `header.php`:

> index.php
```php
<?php
$titulo = "Página inicial";
include("header.php");
echo "<p>Este é o conteúdo da página inicial.</p>";
include("footer.php");
?>
```

## Tratando requisições HTTP

Como todo o processamento da linguagem PHP ocorre do lado do servidor, a única forma de um usuário ou outra aplicação interagir com uma aplicação PHP é por meio de requisições HTTP.

Para uma introdução ao assunto, basta que conheçamos os métodos GET e POST. Caso não os conheça ou não se lembre do assunto, utilizaremos o GET sempre que desejarmos realizar uma consulta ao servidor, lembrando que o método GET expõe as variáveis enviadas na URL da requisição. O método POST será utilizado sempre que o objetivo for modificar algum recurso no servidor ou se os dados enviados não puderem ser exibidos na URL, como em um formulário de login, por exemplo.

Uma requisição HTTP pode ser originada de diversas fontes, como um formulário, um script construído com JavaScript ou simplesmente pela busca de uma URL na barra de endereços do navegador. Esta última obrigatoriamente será uma requisição do tipo GET.

Caso a requisição deva partir de um formulário, esse formulário deverá ser preparado da seguinte maneira, onde o atributo `action` leva a URL do arquivo PHP que realizará o tratamento da requisição, e o atributo `method` especifica se será utilizado o método GET ou POST:

> index.php
```php 
<form action="processar.php" method="get"> 
  <input type="text" name="mensagem"> 
  <input type="submit"> 
</form>
```

> É importante que os campos do formulário possuam o atributo `name` informado, pois é a partir dele que identificaremos os campos no momento do tratamento da requisição.

Quando o usuário realizar o envio deste formulário, o arquivo `processar.php`, presente no servidor, deverá receber os dados preenchidos e realizar o tratamento da requisição. A partir daí há infinitas possibilidades do que o servidor pode fazer com os dados recebidos, mas, em grande parte das vezes, esses dados serão armazenados em um banco de dados.

### Tratando uma requisição HTTP GET

Dando continuidade ao exemplo, um arquivo `processar.php` que receba e manipule os dados de uma requisição HTTP GET poderá ser construído da seguinte forma:

> processar.php
```php
<?php
if(isset($_GET['mensagem'])) {
  echo "A mensagem enviada foi: {$_GET['mensagem']}.";
}
?>
```

No exemplo acima, fizemos o uso da função `isset()`, que verifica se a variável passada por parâmetro foi definida, e da superglobal `$_GET`.

Uma superglobal é uma variável (ou constante) disponível a partir de qualquer lugar do código, sem que necessariamente seja necessário realizar sua inclusão. Observe que não a declaramos antes, nem realizamos sua inclusão, mas podemos referenciá-la sem qualquer problema.

A superglobal `$_GET` é um array associativo que mantém os dados da requisição HTTP GET que originou a execução do script atual. Cada índice deste array irá referenciar uma das variáveis (um dos campos de formulário, neste caso) enviadas na requisição.

Se sua aplicação estiver sendo executada em `http://localhost:8080`, por exemplo, ao enviar o formulário de `index.php`, você verá a seguinte URL em sua barra de navegação: `http://localhost:8080/processar.php?mensagem=SUA_MENSAGEM`. Como previsto, a variável `mensagem` é exposta na URL. Caso houvesse mais variáveis na requisição, elas apareceriam acompanhadas de um `&`; algo como `http://localhost:8080/processar.php?mensagem=SUA_MENSAGEM&usuario=SEU_USUARIO`.

Como dito antes: uma requisição HTTP GET pode partir de uma simples busca a uma URL na barra de endereços do navegador. Se desejar realizar novos testes no `processar.php`, você pode apenas digitar a URL com a mensagem desejada na barra de endereços do navegador, sem ter que preencher novamente o formulário em `index.php`. Insira a seguinte URL na barra de endereços do navegador e tecle <kbd>Enter</kbd> para ver o resultado: `http://localhost:8080/processar.php?mensagem=Olá, mundo`.

### Tratando uma requisição HTTP POST

O tratamento de requisições HTTP POST é muito semelhante ao de requisições GET. No entanto, não poderemos originar uma requisição POST apenas buscando uma URL. Ela terá de partir do nosso formulário. Modifique os arquivos `index.php` e `processar.php` da seguinte forma:

> index.php
```php 
<form action="processar.php" method="post"> 
  <input type="text" name="mensagem"> 
  <input type="submit"> 
</form>
```

> processar.php
```php
<?php
if(isset($_GET['mensagem'])) {
  echo "GET: A mensagem enviada foi: {$_GET['mensagem']}.";
}
if(isset($_POST['mensagem'])) {
  echo "POST: A mensagem enviada foi: {$_POST['mensagem']}.";
}
?>
```

Agora seu formulário enviará requisições utilizando o método POST, mas ainda será possível enviar requisições via GET ao `processar.php`.

### Checando a existência de múltiplas variáveis POST ou GET

Em algumas situações você enviará um conjunto com muitas variáveis por meio de requisições GET ou POST. Imagine um formulário de cadastro de usuário, com 10 ou mais campos. Uma forma mais simples de verificar se todos esses campos foram enviados corretamente é criando um array que represente a lista de campos esperados e comparando esse array com a requisição recebida:

```php
$esperados = array('nome', 'cpf', 'email', 'telefone', 'endereco', 'nascimento');
if (!array_diff($esperados, array_keys($_POST))) {
  echo "Os arays não são diferentes. Todos os campos esperados foram recebidos.";
}
```
No exemplo acima, a função `array_keys()` é utilizada para obter um array contendo os nomes dos campos recebidos na requisição POST. A função `array_diff()` é utilizada para verificar se o array `$esperados` é diferente do array gerado por `array_keys()`. 

## Enviando arquivos para o servidor
### Preparando o formulário
O formulário deve usar enctype="multipart/form-data" para indicar que pode haver arquivos na requisição.

```php 
<form action="processar.php" method="post" enctype="multipart/form-data"> 
  <input type="file" name="imagem"> 
  <input type="submit"> 
</form>
```

### Tratando a requisição
```php 
<?php
$diretorioDeImagens = $_SERVER["DOCUMENT_ROOT"].'/imagens/'; 
$caminhoTemporario = $_FILES['imagem']['tmpname'];
$caminhoDefinitivo = $diretorioDeImagens . basename($_FILES['imagem']['name']);

if (move_uploaded_file($caminhoTemporario, $caminhoDefinitivo)) {
    echo "Arquivo válido e enviado com sucesso.\n";
} else {
    echo "Erro ao armazenar o arquivo.\n";
}
?> 
```
