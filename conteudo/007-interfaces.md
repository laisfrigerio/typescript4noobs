# Interfaces

As interfaces são uma outra forma de declarar um objeto com um nome associados (além de usar o tipo aliases `type`):

- O TypeScript fornece mensagens de erros mais legíveis;
- Tem melhor desempenho;
- E tem melhor interopabilidade com as classes.

## Apelidos de tipo versus interfaces

Usando `type`:

```ts
type Poet = {
  name: string;
  born: number;
};
```

Usando `interface`:

```ts
interface Poet {
  name: string;
  born: number;
}
```

Identicas, não é mesmo?

Mas existem algumas diferenças essenciais entre `interfaces` e `types`:

- As interfaces podem se “mesclar” para ficarem maiores (cenas dos próximos capítulos);
- As interfaces podem ser usadas na verificação do tipo da estrutura de declarações de classe, o que os apelidos de tipos `types` não fazem;
- Geralmente, o verificador de tipos é mais rápido no trabalho com interfaces: elas declaram um tipo nomeado que pode ser armazenado com mais facilidade internamente em cache, em vez de usar uma cópia dinâmica de um novo objeto literal como fazem os apelidos de tipo.

## Propriedades opcionais

Assim como ocorre com os tipos de objetos, nem todas as propriedades das interfaces precisam ser obrigatórias no objeto:

```ts
interface Book {
  author?: string;
  pages: number;
}

// Ok
const book: Book = {
  author: "Jane Austen",
  pages: 100,
};

const missing: Book = {
  author: "Jane Austen",
};
// Error: Property 'pages' is missing in type
// '{ author: string; }' but required in type 'Book'
```

## Propriedades de somente leitura

É possível impedir que usuários de sua interface reatribuam as propriedades dos objetos que a estejam usando.

Assim, ao adicionar o modificador `readonly` antes de uma propriedade, uma vez definido um valor a mesma, ela não deve ser atribuida com um valor diferente:

```ts
interface Book {
  readonly author: string;
  pages: number;
}

function read(book: Book) {
  // Ok: reading the author property does not attempt to modify it
  console.log(book.author);

  book.author = "Dan Brown";
  // Error: Cannot assign to 'author'
  // because it is a read-only property.
}
```

## Interfaces aninhadas

As `interfaces` também permitem a construção de objetos com proprieidades aninhadas (assim como para os `types`):

```ts
interface Author {
  name: string;
  country: string;
}

interface Book {
  name: string;
  author: Author;
}

let book: Book;

// Ok
book = {
  name: "Inferno",
  author: {
    name: "Dan Brown",
    country: "EUA",
  },
};

book = {
  name: "Inferno",
  author: {
    name: "Dan Brown",
  },
};
// Error: Property 'country' is missing in type
// '{ name: string; }' but required in type 'Author'
```

## Extensões de interfaces

Pode ocorrer de termos várias interfaces semelhantes, com as mesmas propriedade.

O TypeScript permite que uma interface `estenda` outra interface, o que a declarará como copiando todas as propriedades da outra interface:

```ts
interface User {
  name: string;
  email: string;
}

interface Employee extends User {
  role: string;
}

// Ok
let employee: Employee = {
  name: "Anne",
  email: "anne@example.com",
  role: "Engineer",
};

let missingEmployee: Employee = {
  name: "Anne",
  email: "anne@example.com",
};
// Error: Property 'role' is missing in type
// '{ name: string; email: string; }' but required in type 'Employee'

let extraProperty: Employee = {
  name: "Anne",
  email: "anne@example.com",
  departament: "IT",
};
// Error: Type '{ name: string; email: string; departament: string; role: string }'
// is not assignable to type 'Employee'
//    Object literal may only specify known properties,
//    and 'departament' does not exists in type 'Employee'
```

> As extensões de interfaces nos permitem evitar digitar o mesmo código repetidamente em várias interfaces para representar esse relacionamento.

É possível estender diversas interfaces simultaneamente:

```ts
interface User {
  name: string;
  email: string;
}

interface Address {
  zipCode: string;
  street: string;
  number: number;
}

interface Employee extends User, Address {
  role: string;
}
```

> Ao marcar uma interface como estendendo outras interfaces, você poderá reduzir a duplicação de códigos e facilitar a reutilização das formas dos objetos em diferentes áreas do código.

## Mesclagem de interfaces

É um recurso das interfaces cuja a capacidade é de mesclar-se umas com as outras. Veja um exemplo em que as interfaces têm o mesmo nome:

```ts
interface Merged {
  fromFirst: string;
}

interface Merged {
  fromSecond: number;
}

// Equivalent to:
// interface Merged {
//	fromFirst: string;
//	fromSecond: number;
// };
```

Esse recurso não é muito utilizável pois pode ser difícil de entender um código em que uma interface é declarada em vários locais.

No entanto, a mesclagem de interfaces é particularmente útil para a ampliação de interfaces de pacotes externos, como `Window`:

```ts
interface Window {
  myEnvironmentVariable: string;
}

window.myEnvironmentVariable; // Type: string
```

As propriedades mescladas NÃO PODEM declarar o mesmo nome de uma propriedade várias vezes com tipos diferentes. Se a propriedade já foi declarada em outra interface, uma interface mesclada terá de usar o mesmo tipo:

```ts
interface User {
  name: string;
  email: string;
  year: string;
}

interface User {
  year: number;
}
// Error: Subsequient property declatations must have the same type.
// Property 'year' must be type of 'string'
// but here has type 'number'
```
