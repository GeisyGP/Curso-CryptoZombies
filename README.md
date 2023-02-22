# Curso-CryptoZombies
Anotações e código de estudo de smart contracts do curso CryptoZombies. O curso ensina Solidity através da criação de um jogo de zumbis.

## 1ª Lição - Making the Zombie Factory

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

## 2ª Lição - Zombies Attack Their Victims

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