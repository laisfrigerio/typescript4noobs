# Funções

Uma função é um bloco de código que realiza uma tarefa específica e pode ser reutilizada em diferentes partes de um programa. Em TypeScript, assim como em outras linguagens, funções podem receber entradas (chamadas de parâmetros), realizar operações e retornar um resultado.

## Parâmetros de funções

```js
function sing(song) {
  console.log(`Singing: ${song}`);
}
```

Com essa sintaxe do JavaScript não é possível inferir com exatidão qual o tipo da variável `song`. Já o TypeScript o considerá de tipo `any`, o tipo de parâmetro que pode ser qualquer coisa.

Assim como nas variáveis, o TypeScript permite declarar o tipo de parâmetros de funções com uma anotação de tipo:

```ts
function sing(song: string) {
  console.log(`Singing: ${song}`);
}
```

## Parâmetros obrigatórios

Ao contrário do JavaScript, que permite que as funções sejam chamadas com qualquer número de argumentos, o TypeScript presume que todos os parâmetros declarados sejam obrigatórios. A contagem de argumentos do TypeScript entra em cena se uma função for chamada com argumentos insuficientes ou em excesso:

```ts
function singTwo(first: string, second: string) {
  console.log(`${first} / ${second}`);
}

singTwo("Rehab");
// Error: Expected 2 arguments, but got 1

singTwo("Rehab", "Hello"); // Ok

singTwo("Rehab", "Hello", "Set Fire to the Rain");
// Error: Expected 2 arguments, but got 3
```

Impor a obrigatoriedade de argumentos para um função ajuda a reforçar a segurança de tipo (`type safety`) assegurando que todos os valores de argumentos esperados estejam dentro da função. Ao contrário, pode resultar em um comportamento inesperado no código, como a função `singTwo` logar `undefined`.

> O parâmetro é a declaração de uma função (o que espera receber). Já o argumento é o valor fornecido a uma função ao invocá-la.

## Parâmetros opcionais

Em casos em que um parâmetro não é obrigatório, podemos explicitamente indicar ao TypeScript parâmetros opcionais. Os parâmetros opcionais não precisam ser fornecidos ao invocar a função e recebem por padrão o o tipo união `| undefined`:

```ts
function announceSong(song: string, singer?: string) {
  console.log(`Song: ${song}`);

  if (singer) {
    console.log(`Singer: ${singer}`);
  }
}

announceSong("Hello"); // Ok
announceSong("Hello", undefined); // Ok
announceSong("Hello", "Adele"); // Ok
```

Qualquer valor opcional deve sempre ficar na última posição. Ao contrário, o TypeScript exibirá um alerta de erro:

```ts
function announceSinger(singer?: string, song: string) {}
// Error: A required parameter cannot follow an optional parameter
```

## Parâmetros rest

Algumas funções do JavaScript são criadas para serem chamadas com qualquer número de argumentos. Para tal usamos o operador spread `...` que é inserido como último parâmetro de uma função. Todos os argumentos passados serão armazenados em um array:

```ts
function singAllSongs(singer: string, ...songs: string[]) {
  for (const song of songs) {
    console.log(`'${song}' by ${singer}`);
  }
}

singAllSongs("Adele"); // Ok
singAllSongs("Adele", "Hello", "Set Fire to the Rain", 5); // Ok
singAllSongs("Adele", 2000);
// Error: Argument of type 'number' is not assignable
// to paramenter of type 'string'
```

## Tipos de retorno

Se o TypeScript conhecer todos os valores que podem ser retornados, saberá que tipo uma função retornará. A propriedade `length` de um array de strings sempre retorna um number:

```ts
// Type: (songs: string[]) => number
function singSongs(songs: string[]) {
  for (const song of songs) {
    console.log(`${song}`);
  }

  return songs.length;
}
```

## Tipos de retorno explicítos

```ts
function singSongs(songs: string[]): number {
  for (const song of songs) {
    console.log(`${song}`);
  }

  return songs.length;
}
```

Com arrow function:

```ts
const singSongs = (songs: string[]): number => {
  for (const song of songs) {
    console.log(`${song}`);
  }

  return songs.length;
};
```

Se uma instrução de return de uma função retornar um valor não atribuível ao tipo de retorno da função, o TypeScript exibirá um alerta de capacidade de atribuição.

```ts
const getSongRecordingDate = (song: string): Date | undefined => {
  switch (song) {
    case "Strange Fruit":
      return new Date("April 10 1939"); // Ok
    case "Greensleeves":
      return "unknown";
    // Error: Type 'string' is not assignable of type 'Date | undefined'
    default:
      return undefined; // Ok
  }

  return songs.length;
};
```

## Retorno void

O tipo de retorno void é usado em TypeScript para indicar que uma função não retorna nenhum valor. Em outras palavras, uma função com retorno void não produz um resultado que possa ser utilizado depois que a função é chamada:

```ts
function helloWorld(): void {
  console.log("Olá, Mundo");
}
```

> Neste exemplo, a função **`helloWorld`** não retorna nenhum valor explicitamente. Ela simplesmente imprime uma mensagem no console, mas não retorna nenhum resultado utilizável.

## Retorno never

Algumas funções não são projetadas nem mesmo para retornar valor, como exemplo, funções que lançam um erro:

```ts
function fail(message: string): never {
  throw new Error(`Invariant failure: ${message}`);
}

function workWithUnsafeParam(param: unknown) {
  if (typeof param !== "string") {
    fail(`param should be a string, not ${typeof param}`);
  }

  // Here, param is known to be type string
  param.toUpperCase(); // Ok
}
```

## Sobrecarga de funções

Sobrecarga de funções é um recurso em TypeScript que permite que você defina múltiplas assinaturas para uma função com o mesmo nome, mas com parâmetros e tipos de retorno diferentes. Isso significa que você pode ter uma única função que se comporta de maneira diferente dependendo dos argumentos que são passados para ela:

```ts
function processarInput(input: string): string;
function processarInput(input: number): number;

function processarInput(input: any): any {
  if (typeof input === "string") {
    return input.toUpperCase();
  } else if (typeof input === "number") {
    return input * 2;
  }
}

console.log(processarInput("hello")); // Saída: "HELLO"
console.log(processarInput(5)); // Saída: 10
```

Neste exemplo, temos três assinaturas para a função `processarInput`. As duas primeiras são as sobrecargas da função:

- Uma espera receber uma string e retorna uma string;
- A outra espera receber um número e retorna um número.

Já a terceira assinatura é a implementação real da função que aceita qualquer tipo de entrada (`any`) e realiza o processamento com base no tipo de entrada.
