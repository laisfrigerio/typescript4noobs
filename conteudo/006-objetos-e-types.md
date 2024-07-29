# Objetos & Types

Um `objeto` é uma entidade que representa uma coleção de propriedades e métodos:

- **Propriedades** são atributos que armazenam dados;
- **Métodos** são funções que operam sobre esses dados.

> Objetos são fundamentais na programação orientada a objetos (OOP), onde são utilizados para modelar entidades do mundo real.

Ao declarar um objeto atribuindo propriedades e valores a ele, o TypeScript cria um novo objeto, ou um novo tipo:

```ts
const poet = {
  born: 1935,
  name: "Mary",
};
```

O TypeScript sabe que o objeto tem 2 propriedades:

- Uma `born` do tipo number;
- Uma `name` do tipo string.

Somente essas duas váriaveis são acessíveis:

```ts
poet["born"]; // Type number
poet.name; // Type string
```

Ao tentar acessar outra propriedade causaria um erro de tipo por esse nome não existir no objeto:

```ts
poet.end;
// Property 'end' does not exist on
// type '{ name: string; born: number }'
```

## Declaração explícita de obejtos

Os objetos podem ser descritos com o uso de uma sintaxe semelhante a dos objetos literais, mas com tipos em vez de valores para os campos:

```ts
let poetLater = {
  born: number;
  name: string;
};

// Ok
poetLater = {
  born: 1935,
  name: "Mary",
};

poetLater = "Joan";
// Error: Type 'string' is not assignable to
// type '{ born: number; name: string }'
```

### Apelidos de tipos

É comum usar alias de tipo para atribuição de um nome a cada forma de tipo.

O trecho anterior poderia ser reescrito com type Poet, o que traria o benefício adicional de tornar a mensagem de erro de capacidade de atribuição do TypeScript mais direta e legível:

```ts
type Port = {
  born: number;
  name: string;
};

let poetLater: Poet;

// Ok
poetLater = {
  born: 1935,
  name: "Mary",
};

poetLater = "Emily";
// Error: Type 'string' is not assignable to 'Poet'
```

### Verificação de uso

Ao atribuir um valor à um variável do tipo objeto, o TypeScript verifica se todas as propriedades obrigatórias estão presentes, se não emitirá uma mensagem de erro:

```ts
type FirstAndLastName = {
  first: number;
  last: string;
};

// Ok
const hasBoth: FirstAndLastName = {
  first: "John",
  last: "Duo",
};

const hasOnlyOne: FirstAndLastName = {
  first: "John",
};
// Property 'last' is missing in type '{ first: string; }'
// but required in type 'FirstAndLastName'
```

Tipos que não sejam compátiveis entre as propriedades também não são permitidos. No exemplo a seguir, o tipo `TimeRange` espera que a propriedade `start` seja do tipo `Date`:

```ts
type TimeRange = {
  start: Date;
};

const hasStartString: TimeRange = {
  start: "2024-01-10",
};
// Error: Type 'string' is not assignable to type 'Date'
```

### Propriedades opcionais

As propriedades de um tipo de objeto não precisam ser todas obrigatórias. Pode incluir o caracter ponto de interrogação `?` antes de `:` na anotação de tipo de uma propriedade para indicar que ela é opcional:

```ts
type Book = {
  author?: string;
  pages: number;
};

// Ok
const bookOne: Book = {
  author: "Dan",
  pages: 420,
};

const missiginBook: Book = {
  author: "Dan",
};
// Error: Type 'pages' is missing on type
//   '{ author: string }' but required in Type 'Book'
```
