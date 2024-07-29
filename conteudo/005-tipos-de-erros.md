# Tipos de erros comuns

Existem 2 erros que são muito comum ao utilizar TypeScript:

- **De sintaxe**, o qual impede que o TypeScript seja convertido em JavaScript
- **De tipo**, quando algo incompátivel é detectado pelo verificador de tipos. Este erro não impedem que a sintaxe TypeScript seja convertida para JavaScript, mas indicará que algo travará ou se comportará inesperadamente se for permitido que o código seja executado.

## Capacidade de atribuição

Uma vez que uma variável é declarada com um tipo ou que o TypeScript aferi seu tipo com base no valor da inicialização, ao ocorrer uma nova atribuição, o verificador de tipos sempre analisará se o novo valor corresponde ao mesmo tipo da variável.

Se ao declarar e inicializar uma variável do tipo String e, posteriormente, tentar atribuir um valor booleano a ela, o TypeScript detectará a atribuição de um tipo diferente e exibirá uma mensagem de erro:

```ts
let lastName = "King";
lastName = true;

// Error: Type 'boolean' is not assignable to type 'string'
```

Esse erro de atribuição será um dos mais comum ao escrever código TypeScript:

```ts
// Error: Type 'boolean' is not assignable to type 'string'
```

O primeiro tipo mencionado nesta mensagem de erro corresponde ao valor que o código está tentando atribuir a um destinatário: `boolean`. O segundo tipo `string` é o tipo atual do destinatário. No seguinte exemplo:

```ts
let lastName = "King";
lastName = true;
// Error: Type 'boolean' is not assignable to type 'string'
```

Estamos tentando atribuir o valor `true` - tipo boolean - à variável de destino lastName - que é do tipo `string`.

## Anotações de tipo

Quando uma variável é declarada no TypeScript sem o tipo ou sem um valor inicial, o verificador de tipos não tentará descobrir o tipo inicial da variável de usos posteriores.

Por padrão, o tipo `any` será inferido, indicando que ela pode ser **qualquer coisa**. Isso implica que o TypeScript permitirá a atribuição de valores a uma variável mesmo sendo de tipos diferentes (algo comum no JavaScript):

```ts
let rocker; // Type any

rocker = "John"; // Type string
rocker.toUpperCase(); // ok

rocker = 19.58; // Type number
rocker.toPrecision(1); // ok

rocker.toUpperCase();
// Error: 'toUpperCase' does not exist on type 'number'.
```

Lembrando que:

> Permitir o uso do tipo any de forma geral, invalida parcialmente a finalidade da verificação de tipos do TypeScript. O TypeScript funciona melhor quando ele sabe que tipos os valores devem ter.

Podemor declarar uma variável sem atribuir valor inicial, mas devemos informar o tipo da variável. Essa anotação é conhecida como `type annotation`:

```ts
let rocker: string;
rocker = "Joan";
```

> Lembrando que essas anotações de tipo apenas existem para o TypeScript. Quando o código TypeScript é transpilado para JavaScript `tsc`, elas são excluídas:

```js
// output .js file
let rocker;
rocker = "Joan";
```

## Anotações de tipos redundantes

A anotação de tipo a seguir é redundante porque o TypeScript pode inferir que `firstName` é de tipo string pela atribuição de valor:

```ts
let firstName: string = "Joan";
```

Ao adicionar a tipagem, o sistema de tipos irá validar se a inicialização corresponde ao valor atribuído:

```ts
let firstName: string = 42;
// Error: Type 'number' is not assignable to type 'string'
```

> Pode ser útil incluir anotações de tipos explícitas para as variáveis com o objetivo de documentar o código de maneira mais clara e/ou para deixar o seu código protegido contra alterações acidentais no tipo da variável.
