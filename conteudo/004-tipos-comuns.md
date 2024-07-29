# Tipos mais comuns

Os tipos mais básicos do TypeScript são os 7 tipos primitivos do JavaScript:

- string
- number
- boolean
- null
- undefined
- bigint
- symbol

```ts
"Louise"; // string
1337; // number
true; // boolean
null; // null
undefined; // undefined
1337n; // bigint
Symbol("John"); // symbol
```

## String

Strings podem ser declaradas usando aspas simples e duplas, além do uso das aspas invertidas para que algumas operações lógicas sejam inseridas dentro da variável. Sua tipagem é escrita como `:string`

```ts
const userName: string = "João";

const greetingMessage: string = `Bom dia! O meu nome é ${userName}!`;
```

## Number

É possível tipar qualquer tipo de número. Sua tipagem é escrita como `:number`

```ts
const average: number = 50;
```

## Boolean

Aceita os valores true e false. Sua tipagem é escrita como `:boolean`

```ts
const isAdmin: boolean = true;
```

## Outros tipos

### Any

O tipo `any` pode ser, como o nome sugere, qualquer coisa. Recomenda-se evitar ao máximo o uso desse tipo já que no final das contas usá-lo seria o mesmo que não usar Typescript.

> É possível configurar seu `tsconfig` para que seu código não aceite tipagens com `any` para uma maior segurança.

```ts
let anything;
anything = 1;
anything = "Anything...";
anything = [1, 2];
```

> Permitir o uso do tipo any de forma geral, invalida parcialmente a finalidade da verificação de tipos do TypeScript. O TypeScript funciona melhor quando ele sabe que tipos os valores devem ter.

### Void

O tipo `void`, geralmente utilizado no retorno de funções, representa uma função que não retorna nenhum valor em especifíco, ou seja, não retorna nada:

```ts
function readFile(): void {
  // read a file
  // do not return anything...
}
```

### Never

O tipo `never` é especialmente útil no gerenciamento de erros quando queremos criar exceções e indicar que nunca irão retornar nada:

```ts
function error(): never {
  throw new Error("errrouuuu");
}
```

### Array

Arrays representam uma coleção de um tipo de dado:

```ts
const studentsGrade: number[] = [7.5, 8, 6.3, 9];
const fruitsName: string[] = ["Apple", "Orange", "Banana"];
const numberList: Array<number> = [1, 2, 3];
```
