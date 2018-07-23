# Jasmine

É um framework javascript usado para realizar testes de comportamento (BDD), podendo ser rodado no navegador, Node, Ruby e Python.

## Instalando o Jasmine

Para instalar o framework, rode o seguinte comando no terminal:

```node
npm install jasmine -g
```

## Iniciando o Jasmine no projeto

Rode o seguinte comando para criar os diretórios padrões que o framework necessita para iniciar a rodar os teste.

```node
jasmine init
```

Com isto, ele ira criar uma estrutura de pastas onde dentro dela estara o arquivo de configuração do Jasmine (jasmine.json), onde poderá ser alterado algumas configurações relacionadas aos testes.
Quando for realizado o teste, o Jasmine ira buscar por padrão os arquivos dentro da pasta "spec", onde que estes devem terminar com o nome "spec.js", por isto, todos os códigos de testes deverão estar dentro deste diretório.

## Executando o Jasmine

* describe(): Esta função cria o que chamamos de "suíte", seu primeiro parâmetro recebe o nome do tipo de teste que vai ser utilizado, normalmente sendo o nome do nosso arquivo, no segundo parâmetro passamos uma função anônima, onde dentro dela será codificada a criação dos testes. Dentro de cada "suíte" podemos ter quantas "specs" quisermos;
* it(): chamada de "especificação" ou "spec" de especification. É uma função que o primeiro parâmetro passamos uma string que indicamos o que um certo pedaço de código deve fazer, descrevendo sua ação. No segundo parâmetro é colocado uma função de callback, que é onde finalmente escreveremos algum teste;
* expect(): Essa função armazena o valor passado no parâmetro, para em seguida utilizar funções variadas de teste como a "toEqual()", que recebe um parâmetro que sera com comparado com o valor armazenado, estas sendo chamadas de "Matchers", onde podemos utilizar as já presentes no framework ou criar as nossas.

Exemplo de teste:

```node
// Arquivos hello.js
function hello(){
    return 'Hello World!';
}
module.exports = hello;
```

```node
// Arquivo hello-spec.js
var hello = require('../hello');

describe('Hello', function(){
    it('has to print "Hello World!"', function(){
        var text = hello();
        expect(text).toEqual('Hello World!');
    })
})
```

Para realizar os testes, basta rodar o comando abaixo na linha de comando e verificar o resultado:

```node
jasmine
```

Resultado por exemplo uma resposta parecida com a abaixo, demonstrando as specs encontradas, falhas ocorridas e o tempo em segundos para execução:

```node
Randomized with seed 16281
Started
.


1 spec, 0 failures
Finished in 0.005 seconds
Randomized with seed 16281 (jasmine --random=true --seed=16281)
```

Caso haja algum erro, a saida da mensagem sera diferente, como por exemplo a resposta abaixo:

```node
Randomized with seed 20064
Started
F

Failures:
1) Hello has to print "Hello World!"
  Message:
    Expected 'Hello Wowrld!' to equal 'Hello World!'.
  Stack:
    Error: Expected 'Hello Wowrld!' to equal 'Hello World!'.
        at <Jasmine>
        at UserContext.<anonymous> (C:\git\course-treina-web-javascript-testes-automatizados-com-jasmine\Teste Jasmine\spec\hello-spec.js:6:22)
        at <Jasmine>

1 spec, 1 failure
Finished in 0.008 seconds
Randomized with seed 20064 (jasmine --random=true --seed=20064)
```

Podemos ver que um bom nome de spec e suíte podem fazer a diferença para que possamos encontrar facilmente o erro a ser corrigido, cmo por exemplo o resultado 'Hello has to print "Hello World!"', onde podemos saber o nome do arquivo ("Hello") e o que ele estava tentando verificar ('has to print "Hello World!"').

## Automatização de execução do Jasmine

Para executarmos automaticamente os teste a cada alteração de código realizada (escutar as alterações), podemos utilizar o Nodemon para realizar esta tarefa para nós. Podemos instala-lo com o comando abaixo, instalando assim globalmente:

```node
npm install nodemon -g
```

Nodemon é uma ferramenta que executa o Node.js e reinicia ele assim que uma modificação em algum arquivo for localizada.

Com o Nodemon instalado, ao invés de simplesmente chamar o Jasmine pelo terminal, executamos o seguinte comando:

```node
nodemon --exec jasmine
```

Com isto o Nodemon executará o Jasmine a cada alteração de algum arquivo no nosso projeto.

## TDD - Test Driven Development com Jasmine

Resumidamente é a prática de escrever os teste primeiro e depois o código. Com isso, podemos definir no teste quais são os requisitos de determinada funcionalidade e seus respectivos retornos. Então, ao criarmos a funcionalidade, os testes automatzados indicarão quando finalmente terminarmos de escrever uma funcionalidade que realmente entrega tudo o que deveria fazer.
Para verificar um exemplo utilizando TDD, verifique os arquivos "anagram-spec.js" e "anagram.js" neste repositório.

## Lista de Matchers

### toEqual

Usado para comparar o valor da função expect com o valor passado por parâmetro na sua função.

```node
var a = 'TreinaWeb';
var b = 'TreinaWeb';

expect(a).toEqual(b); // passa no teste
```

### toBe

Igual ao toEqual(), porem contem uma pequena diferença. Quando fazemos comparação com objetos, o toBe verifica se ambos são o mesmo objeto não importando se são simplesmente iguais.

```node
var a = {nome: 'TreinaWeb'};
var b = {nome: 'TreinaWeb'};

expect(a).toEqual(b); // passa pois os osjetos são iguais
expect(a).toBe(b);    // dará erro, pois os objetos, mesmo sendo iguais, não são os mesmo.
expect(a).toBe(a);    // dará sucesso, pois os objetos testados são os mesmos.
```

### toBeTruthy e toBeFalsy

Servem para verificar se um valor é verdadeiro ou falso.

```node
expect("TreinaWeb").toBeTruthy();
expect(0).toBeFalsy();
```

### not

Podemos realizar a negação de um matcher utilizando o matcher not

```node
expect(false).not.toBeTruthy();
```

### toContain

Usado para verificar se um determinado elemento está presente no meio de outros elementos. Isso serve tanto para um item dentro de uma lista quanto para caracteres dentro de uma string.

```node
expect([1,2,3]).toContain(2);
expect("TreinaWeb").toContain("Web");
expect("Hello Web").not.toContain("World");
```

### toBeDefined e toBeUndefined

Verifica se determinado elemento está definido.

```node
var obj = {};
expect(obj.name).toBeUndefined();
obj.name = "TreinaWeb";
expect(obj.name).toBeDefined();
```

### toBeNull

Verifica se algo é nulo.

```node
expect(null).toBeNull();
```

### toBeNan

Verifica se algo é NaN (not a number).

```node
expect(10).not.toBeNaN();
expect(0 / 0).toBeNaN();
```

OBS: O Javascrip possui uma função nativa que verifica se algo é NaN. A diferença é que a função do Javascript retornará verdadeiro para qualquer coisa que não seja um número ou string numérica, enquanto a função do Jasmine só retorna true se o valor passado for exatamente NaN.

### toBeGreaterThan e toBeLessThan

Usado para verificar se algo é maior ou menor que o valor passado. Essa função funciona para strings tambem.

```node
expect(10).toBeGreaterThan(1);
expect(1).toBeLessThan(10);
expect('a').toBeLessThan('z');
```

### toBeCloseTo

Verifica se um número é próximo de outro número, passando uma quantia de precisão de casas decimais para indicar se pode ser considerado próximo ou não.

```node
expect(20.3).toBeCloseTo(20.32, 1);
```

### Matchers com Expressões Regulares

```node
expect("TreinaWeb").toMatch(/Web/);
```

### toThrow

Faz com que esperemos que uma função retorne um erro.

```node
var myFunction = function(){
    throw new Error();
}
expect(myFunction).toThrow();
```
