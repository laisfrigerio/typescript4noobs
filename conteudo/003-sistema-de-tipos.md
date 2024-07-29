# Sistema de tipos

> O poder do JavaScript vem da flexibilidade. Cuidado com isso!

## Verificador de tipos

O verificador de tipos é responsável por ler o código, examinar, entender seu funcionamento e informar onde podemos ter errado. Mas como uma verificador de tipos funciona na prática?

O tipo é uma descrição de um valor do código:

```ts
const singer = “Beyonce”;
```

Neste exemplo, o TypeScript poderá inferir, ou entender, que a variável `singer` é de tipo string.

## Sistema de tipos

Portanto, o sistema de tipos é um conjunto de regras que permite que uma linguagem de programação saiba quais tipos as estruturas de um programa pode ter.

Basicamente, o sistema de tipos funciona da seguinte maneira:

- Lê e detecta todos os tipos e valores que existem;
- Para cada valor, verifica o tipo com base na sua declaração inicial;
- Para cada valor, análise todas as maneiras de como ele é usado no código;
- Alerta o usuário se o uso de um valor não correspondente ao tipo.

Conseidere o seguinte exemplo:

```ts
let firstName = "Nick";
firstName.length();

//
// This expression is not callable
//   Type 'Number' has no call signatures
//
```

O TypeScript chegou a esse alerta da seguinte maneira:

- Fez a leitura do código e identificou que há uma variável chamada `firstName`;
- Concluiu que `firstName` é do tipo string porque seu valor inicial é a string `Nick`
- Fez análise e verificou que o código está tentando acessar a propriedade `.length` de `firstName` e chamá-la como uma função
- Por fim, alerta que `.length` é uma propriedade do tipo `number` e não uma função (_não pode ser chamado como uma função_)

> As anotações de tipo apenas existem para o TypeScript. Quando o código TypeScript é transpilado para JavaScript com o compilador `tsc`, elas são excluídas.

Código TypeScript:

```ts
// output .ts file
let rocker: string;
rocker = "Joan";
```

Compilado:

```js
// output .js file
let rocker;
rocker = "Joan";
```
