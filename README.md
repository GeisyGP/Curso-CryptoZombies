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