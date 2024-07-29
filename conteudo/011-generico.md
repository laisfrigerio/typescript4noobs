# Genérico

Existem trechos de códigos que precisam operar com tipos diferentes dependendo de como for usado. Na função do exemplo a seguir, qual seria o seu tipo de parâmetro e de retorno?

```js
function identity(input) {
  return input;
}

identity("abc");
identity(123);
identity({ message: "Hi!" });
```

Poderiamos declar o input como `any`, mas então o tipo de retorno da função também seria `any`:

```ts
function identity(input: any) {
  return input;
}

let value = identity(42); // Type of value: any
```

Já que `input` pode ser qualquer entrada, precisamos de uma maneira de informar que há um relacionamento entre o tipo de input e o tipo que a função retorna. O TypeScript captura relacionamento entre tipos usando `genéricos`.

## Estruturas que podem se tornar genéricas

### 1 - Funções genéricas

Para tornar uma função genérica inserimos um alias de parâmetro de tipo e assim o TypeScript pode inferir um tipo diferente para T sempre que a função for chamada:

```ts
function identity<T>(input: T) {
  return input;
}

const numeric = identity(123); // Type: 123
const stringy = identity("me"); // Type: "me"
```

As funções podem definir qualquer número de parâmetroas de tipo separados por virgula:

```ts
function makeTuple<First, Second>(first: First, second: Second) {
  return [first, second] as const;
}

// Type of value: readonly [boolean, string]
let tuple = makeTuple(true, "abc");
```

É importante ressaltar que se uma função declarar múltiplos parâmetros de tipo, as chamadas a ela devem declarar explicítamente todos os tipos genéricos ou não declarar nenhum deles:

```ts
function makePair<Key, Value>(key: Key, value: Value) {
  return { key, value };
}

// Ok: Neither type argument provided
makePair("abc", 123); // Type: { key: string, value: number }

// Ok: Both type arguments provided
makePair<string, number>("abc", 123); // Type: { key: string, value: number }
makePair<"abc", 123>("abc", 123); // Type: { key: "abc, value: 123 }

makePair<string>("abc", 123);
// Error: expected 2 type arguments, but got 1.
```

> Evite o uso de 3 ou mais tipos em uma estrutura genérica. Assim como ocorre com os parâmetros de funções no runtime, quanto maior for a quantidade de parâmetros, mas difícil será de ler e entender o código.

### 2 - Interfaces Genéricas

As interfaces também podem ser declaradas como genéricas. Seguem regras semelhantes das funções: podem ter qualquer número de parâmetros de tipo declarados entre `< e >` após seu nome:

```ts
interface Box<T> {
  inside: T;
}

let stringyBox: Box<string> = {
  inside: "abc",
};

let numberBox: Box<number> = {
  inside: 123,
};

let incorrectBox: Box<number> = {
  inside: false,
  // Error: Type 'boolean' is not assignable to type 'number'.
};
```

Por exemplo, o TypeScript usa `generics` para especificar o tipo dos elementos que um array pode conter:

```ts
let array: Array<number> = [1, 2, 3];
```

Neste caso, o tipo **`Array<number>`** indica que **`array`** é um array que contém apenas números. Isso fornece uma verificação de tipo estática durante o tempo de compilação e ajuda a evitar erros de tipo em tempo de execução.

A implementação da Interface genérica de um `Array` ficaria algo como:

```ts
interface Array<T> {
  length: number;
  push(...items: T[]): number;
  pop(): T | undefined;
  concat(...items: T[]): T[];
  // Outros métodos do array...
}
```

### 3 - Classes genéricas

As classes também podem declarar qualquer número de parâmetros de tipo:

```ts
class Secret<Key, Value> {
  key: Key;
  value: Value;

  constructor(key: Key, vaue: Value) {
    this.key = key;
    this.value = value;
  }

  getValue(key: Key): Value | undefined {
    return this.key === key ? this.value : undefined;
  }
}

// Type: Secret<number, string>
let storage = new Secret(12345, "password");
storage.getValue(1987); // Type: string | undefined
```

As classes genéricas podem ser usadas como classe base. Neste caso, o TypeScript não tentará inferir argumentos de tipo para a classe base, ou seja, ao estender uma classe genérica, você deve fornecer tipos para todos os parâmetros genéricos da classe base:

```ts
class Box<T> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }
}

class BoxWithLog<T> extends Box<T> {
  log() {
    console.log(`Value: ${this.value}`);
  }
}

const boxWithNumber = new BoxWithLog<number>(42);
boxWithNumber.log();
```

Ou

```ts
class Quote<T> {
  lines: T;

  constructor(lines: T) {
    this.lines = lines;
  }
}

class SpokenQuote extends Quote<string[]> {
  constructor(lines: T) {
    super(lines);
  }

  speak() {
    console.log(this.lines.join("\n"));
  }
}

new Quote("abc"); // Type String
new Quote([4, 8, 7, 1]); // Type: number[]

new SpokenQuote(["Abc", "Def"]).lines; // Type: string[]

new SpokenQuote([1, 2, 4, 5]);
// Error: Argument of type 'number[]' is not
// assignable to parameter of type 'string[]'
```

### 4 - Aliases de tipo genérico

Os tipos aliases também podem se tornarm genéricos. Cada alias de tipo pode receber qualquer número de parâmetros de tipo:

```ts
type Nullish<T> = T | null | undefined;
```

Normalmente, os aliases de tipo genérico são usados em funções para descrever o tipo de uma função genérica:

```ts
type CreateValue<Input, Output> = (input: Input) => Output;

let creator: CreateValue<string, number>;

creator = (text) => text.length; // Ok

creator = (text) => text.toUpperCase();
// Error: type 'string' is not assignable to type 'number'
```

## Promise

Promises, no JavaScript, representa algo que ainda pode estar pendente, como uma solicitação de rede. Cada Promise fornece métodos para o registro de callbacks para o caso de a ação pendente ser “resolvida” (se concluída com sucesso) ou ser “rejeitada” (lançar um erro).

No TypeScript as promises são respresentadas na tipagem como uma classe **`Promise`** com um único parâmetro de tipo representando o eventual valor resolvido.

### Criação de promises

O construtor de Promise é tipado como recebendo um único parâmetro. O tipo desse parâmetro depende de um parâmetro de tipo declarado na classe genérica Promise. Uma forma reduzida teria essa aparência:

```ts
class PromiseLike<Value> {
  constructor() {
  	executor: (
  		resolve: (value: Value) => void,
  		reject: (reason: unknown) => void,
  	) => void,
  };
};
```

A criação de uma promise destinada a ser resolvida com um valor geralmente demanda a declaração explicíta do argumento de tipo da promise. Por padrão, o TypeScript presumirá que o tipo do parâmetro é `unknown` se não houver esse argumento de tipo genérico explicíto:

```ts
// Type: Promise<unknown>
const resolvesUnknown = new Promise((resolve) => {
  setTimeout(() => resolve("Done!"), 1000);
});

// Type: Promise<string>
const resolvesString = new Promise<string>((resolve) => {
  setTimeout(() => resolve("Done!"), 1000);
});
```

### Funções async

Qualquer função declarada em JavaScript com a palavra reservada `async` retorna uma `Promise`. Assim, o TypeScript inferirá o tipo de retorno de uma função async como sempre sendo uma Promise para qualquer que seja o valor retornado.

A seguir, a função `lengthAfterSecond` retorna uma `Promise<number>`, enquanto `lengthImmediately` é inferida como retornando uma `Promise<number>` porque é assíncrona e retorna diretamente number:

```ts
// Type: (text: string) => Promise<number>
async function lengthAfterSecond(text: string) {
  await new Promise((resolve) => setTimeout(resolve, 1000));
  return text.length;
}

// Type: (text: string) => Promise<number>
async function lengthImmediately(text: string) {
  return text.length;
}
```

Portanto, qualquer tipo de retorno declarado manualmente em uma função `async`, deve sempre ser de tipo `Promise`, mesmo se a função não mencionar promises explicítamente em sua implementação:

```ts
// Type: () => Promise<string>
async function givesPromiseForString(): Promise<string> {
  return "Done!";
}

async function givesString(): string {
  return "Done!";
}
// Error: The return type of an async function
// or method must be the global Promise<T> type.
```

## Convenções de nomeação dos genéricos

1. A convenção de nomeação padrão para parâmetros de tipo em muitas linguagens é chamar o primeiro argumento de tipo de `T` (que vem de `tipo` ou `template`) e, se existirem parâmetros de tipos subsequentes, chamá-los de U, V, e assim por diante.

2. Quando alguma informação contextual é conhecida sobre como o argumento de tipo deve ser usado, às vezes a convenção é estendida para a utilização da primeira letra do termo desse uso. Por exemplo: bibliotecas de gerenciamento de estados podem se referir a um estado genérico usando `S`. `K` e `V` com frequência se referem as chaves (`keys`) e valores (`values`) nas estruturas de dados.

> Sempre que uma estrutura tiver vários parâmetros de tipos, ou se a finalidade de um único argumento de tipo não ficar imediatamente clara, considere usar nomes por extenso para melhorar a legibilidade em vez de abreviações de letra única.
