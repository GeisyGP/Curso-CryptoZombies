# Curso-CryptoZombies
Anotações e código de estudo de smart contracts do curso CryptoZombies. O curso ensina Solidity através da criação de um jogo de zumbis.

## 1ª Lição - Fazendo a Fábrica de Zumbi

Inicia com a função pragma, especificando que versão de solidity irá ser usada no contrato. Cria o contrato declarando `contract ZombieFactory{}`.


- As variáveis ​​de estado são gravadas no blockchain Ethereum.
- As structs permitem criar tipos de dados mais complicados com várias propriedades.
- `public` cria dados públicos, outras pessoas podem ver mas não podem alterar.
- `private` apenas outras funções dentro do contrato poderão chamar essa função e adicionar à matriz de números (por convenção funções privadas começam com `_`);
- Arrays podem ser fixos (`uint[2] fixedArray`) e dinamicos (`uint[] dinamicArray`).

> Em `Zombie[] public zombies;`, Zombie é o tipo (struct) do array e zombies é o nome do array.

- Em uma função quando passar parâmetros pode existir dois tipos: por valor (em que é feito uma cópia do valor na função) e por referência (ela é chamada com uma referência a variável original, sendo possível alterar seu valor). Quando é por valor, usa-se `memory`.

> Existe uma convenção de iniciar o nome das variáveis de parâmetro com `_`.

- Para criar e adicionar ao array em uma mesma linha: `people.push(Person(16, "Vitalik"));`
- Uma função declarada com `view` significa que vê os dados mas não os modifica. Já uma função declarada com `pure` depende apenas de seus parâmetros, ela não lê o estado da aplicação.
- Para criar funções "pseudo aleatórias" usa-se a função hash **keccak256**.
- **Events**: são uma forma do contrato comunicar que algo aconteceu no blockchain para o front-end do aplicativo, que pode "escutar" certos eventos e agir quando eles acontecerem.

## 2ª Lição - Zumbis atacam suas vítimas

- Além de array e struct, **mapping** é outra forma de armazenar e organizar dados: `mapping = (tipo da chave => tipo do valor) nome;`

> Para armazenar a propriedade do zumbi, vamos usar dois mapeamentos: um que controla o endereço do dono de um zumbi e outro que controla quantos zumbis um proprietário possui.

- A variável global `msg.sender` refere-se ao endereço da pessoa ou smartcontract que chamou a função.
- `require` faz com que a função mostre um erro e interrompa a execução se a condição não for verdadeira.
- Quando um contrato se torna longo demais, pode dividi-lo em outros contratos. Usando "herança" temos por exemplo: `contract ZombieFeeding is ZombieFactory {}`. 
- Existem duas formas de armazenar variáveis:
  - Storage: variáveis ​​armazenadas permanentemente no blockchain;
  - Memory: são temporárias e são apagadas entre chamadas de funções externas para o contrato.
- Além de existir `public` e `private` para definir uma função, existem mais dois tipos de visibilidade `internal` e `external`:
  - `internal` é o mesmo que `private` exceto que é também acessível para contratos que herdam a partir do contrato.
  - `external` é o mesmo que `public` exceto que essas funções podem ser chamadas somente fora do contrato.
- Para usar dados de um contrato externo, temos que definir a interface. Para isso usamos `contract KittyInterface {}`, e dentro copiamos a função com um `;` (ponto e vírgula) após a declaração de returns.
- Uma função como `getKitty` retorna vários valores, para usar um específico usamos vírgulas: `(,,,c) = getKitty();`. Nesse caso o quarto valor será atribuído a variável `c`.
- Para adicionar dois digitos finais:
  - `newDna = newDna - newDna % 100 + 99;`
  - Explicação: "suponha que newDna é 334455. Então newDna % 100 é 55, então newDna - newDna % 100 é 334400. Finalmente adicione 99 para ter 334499."

## 3ª Lição  - Conceitos Avançados de Solidity

Por ser external o contrato tem uma falha de segurança, para resolver isso podemos tornar o contrato **Ownable** (significa que ele tem um dono). 

Para usar **Ownable** é preciso criar um novo contrato e copiar o código que consta em `ownable.sol` e também está disponível na biblioteca [OpenZeppelin](https://www.openzeppelin.com/).

- Função modificadora personalizável tem uma palavra reservada `modifier`. Ela não pode ser chamada diretamente como uma função, mas pode ser usada no final de uma definição de função para mudar seu comportamento.
- No geral definir o tamanho de uma variável (uint16) não muda o custo de gás. Porém em structs sim.
- Solidity oferece unidades de tempo:
  - `now`: retorna unix timestamp atual (o número de segundos que passou desde 1 de Janeiro de 1970);
  - `1 days` : 86400 segundos, assim como minutos, horas e semanas.
- Função `view` são de graça quando chamadas externamente.
- Usar `storage` é uma das operações mais cara em Solidity.
- Diferente de arrays em armazenamento (storage), **arrays em memória** tem que ser criado com tamanho.
  -  Em `uint[] memory result = new uint[](ownerZombieCount[_owner]);` o tamanho do array é definido por(ownerZombieCount[_owner]).

## 4ª Lição - Sistema de Batalha Zumbi

- Outra função modificadora utilizada é o `payable`:
  - `msg.value` (pode transferir Ether)
  - `transfer` (transfere Ether para um endereço)
  - `this.balance` (saldo total do contrato)
  
No StackOverflow há ideias de (como gerar um número aleatório seguro)[https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract]. Como no jogo não será realizada grandes transações, é usado o keccak256:

- Ele será "alimentado" por: `now, msg.sender, randNonce` em que randNonce é um valor incrementado.
 
## 5ª Lição - ERC721 & Cripto-Colecionáveis 

Tokens ERC20 são capazes de interagir com qualquer outro token ERC20, funciona bem quando agem como moedas. Porém em um jogo de zumbi não faz sentido enviar 0.34 de um zumbi. Existe outro padrão de token, o ERC721 melhor para cripto-colecionáveis.

> "Tokens ERC721_ não são intercambiáveis uma vez que cada um é suposto para ser único, e não divisíveis. Você somente pode trocá-los em unidades inteiras, e cada um tem um ID único. Então esses se encaixam perfeitamente para fazer nossos zumbis trocáveis."

- **Overflow** (transbordamento) é quando se tem por exemplo, um uint8, ou seja, só pode guardar (2^8)-1 = 255. Se atribuido 256 a uma variável de 8 bits, ela se torna igual a 0 (como as horas, 23:59, depois 00:00).
- **Underflow** é onde você subtrai: 0 - 1, de um uint8 e vai dar igual a 255.

Para não acontecer esses erros, OpenZeppelin criou uma library (biblioteca) chamada SafeMath. Bibliotecas são parecidas com contratos, mas com o uso da palavra `using` ela implementa todos os métodos em outro tipo de dado. 

- A diferença de `require` e de `assert` é que o primeiro irá reembolsar o usuário o resto do seu gás quando a função falhar, enquanto que assert não irá.
  
Para comentar um código em Solidity usa-se o padrão natspec:
- @title e @author são simples.
- @notice explica para o usuário o que o contrato / função faz. 
- @dev é para explicar detalhes extras para os desenvolvedores.
- @param e @return são para descrever o que cada parâmetro e valor de retorno da função fazem. 