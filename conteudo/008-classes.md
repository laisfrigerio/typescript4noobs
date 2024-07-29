# Classes

O JavaScript atual (2024) é muito diferente comparado com sua versão dos anos 2010. Os comandos `let` e `const`, por exemplo, ainda não existiam. Não tínhamos transpiladores, como `Babel`, que converte uma versão mais recente do JavaScript para uma suportada pela grande maioria dos browsers. As Classes são um conceito que hoje existe no JavaScript vanila, mas também não tinham sido criadas ainda naquela época [2010].

Muita coisa evoluiu. JavaScript passou uma grande evolução nos últimos 10 anos. Hoje temos funcionalidades e recursos bem avançados e interessantes, que já são aceitas por grande parte dos navegadores de forma nativa. E as classes são uma delas!!

## Declarando classes

Declarando uma classe Student com um atributo name, get and set:

```ts
class Student {
  this.name: string;

  setName(name: string): void {
  	this.name = name;
  };

  getName(): string {
  	return this.name;
  };
};

const student: Student = new Student();
student.setName("Alice"); // Ok
student.getName(); // Alice

new Student().setName();
// Error: Expected 1 arguments, but got 0.
```

O construtor é tratado como método típico de uma classe. Uma vez definido, o sistema de tipo será acionado a cada instanciação da classe para verificar que os argumentos estão sendo passados corretamente:

```ts
class Student {
  this.name: string;

  constructor(name: string) {
    this.name = name;
  };

  setName(name: string): void {
  	this.name = name;
  };

  getName(): string {
  	return this.name;
  };
};

const student: Student = new Student("Alice"); // Ok

new Student();
// Error: Expected 1 arguments, but got 0.
```

## Propriedades de classes

As propriedades das classes precisam ser declaradas explicitamente. A sintaxe é parecida com as propriedades das interfaces:

```ts
class Student {
  this.name: string;
  this.email: string;
};
```

Todas as propriedades que vão ser acessadas dentro do método construtor precisam ser declaradas explicitamentes:

```ts
class Student {
  this.name: string;
  this.email: string;

  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
    this.birth = new Date();
    // Error: Property 'birth' does not exist on type 'Student'
  };
};
```

A tentativa de acesso à propriedades que não foram explicitamente declaradas também resultará em um alerta de erro:

```ts
class Student {
  this.name: string;
  this.email: string;

  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
  };
};

const student: Student = new Student("Anne", "anne@example.com");
student.birth;
// Error: Property 'birth' does not exist on type 'Student'
```

## Propriedades de função

Existem duas maneiras de declarar funções dentro de uma classe no TypeScript: como método e como propriedade:

- Como método;
- Como propriedade.

### Como método

O método é compartilhado, ou seja, a mesma implementação é compartilhada entre todas as instâncias da classe:

```ts
class WithMethod {
  myMethod() {}
}

new WithMethod().myMethod === new WithMethod().myMethod; // true
```

### Como propriedade

O método é único por instância da classe. Isso significa que é possível usar a referência `this` para acionar propriedades da classe:

```ts
class WithProperty {
  myProperty: () => {};
}

new WithProperty().myProperty === new WithProperty().myProperty; // false
```

## Verificação de inicialização

O TypeScript checa se todas as propriedades foram inicializadas com um valor.

As propriedades quando são inicializadas no método construtor:

```ts
class Student {
  this.name: string;
  this.email: string;

  constructor(name: string, email: string) {
  	this.name = name;
  	this.email = email;
  };
};
```

As propriedades quando sãoinicializadas na declaração (as demais foram inicializadas no construtor):

```ts
class Student {
  this.name: string;
  this.email: string;
  this.department: string = "general";

  constructor(name: string, email: string) {
  	this.name = name;
  	this.email = email;
  };
};
```

Também é possível utilizar o tipo união `| undefied`. Automaticamente a propriedade será inicializada com valor `undefined` caso nada lhe seja atribuido (seja na declaração ou dentro do método construtor):

```ts
class Student {
  this.name: string;
  this.email: string;
  this.department: string | undefined;

  constructor(name: string, email: string) {
  	this.name = name;
  	this.email = email;
  };
};
```

Caso nenhuma das formas de inicialização sejam aplicadas, o verificador de tipos exibirá um alerta:

```ts
class Student {
  this.name: string;
  this.email: string;
  this.department: string;
  // Error: Property 'department' has no initializer
  // and is not definitely assigned in the constructor

  constructor(name: string, email: string) {
  	this.name = name;
  	this.email = email;
  };
};
```

## Propriedades opcionais

Assim como nos aliases de tipo e interfaces, as classes também podem possuir propriedades opcionais. Automaticamente seu tipo é de união incluindo `| undefined` e a verificação de inicialização não será acionada para propriedades opcionais:

```ts
class Student {
  this.name: string;
  this.email: string;
  this.department?: string;

  constructor(name: string, email: string) {
  	this.name = name;
  	this.email = email;
  };
};

new Student("Anne", "anne@example.com").department?.length; // Ok
```

## Propriedades somente leitura

Novamente, as classes também podem ter propriedades somente de tipo leitura, como nas interfaces:

```ts
class Quote {
  readonly this.text: string;

  constructor(text: string) {
  	this.text = text;
  };

  emphasize() {
  	this.text += "";
  	// Error: Cannot assign to 'text' because it is a read-only property
  };
};

const quote = new Quote("Home sweet Home");

Quote.text = "Ha!";
// Error: Cannot assign to 'text' because it is a read-only property
```

## Classes como tipos

As classes são relativamente únicas na tipagem já que uma declaração de classe cria tanto um valor de runtime como um tipo que pode ser usado em anotações de tipo:

```ts
class Student {
  this.name: string;

  constructor(name: string) {
  	this.name = name;
  };
};

let student: Student;

student = new Student("Anne", "anne@example.com"); // Ok

student = "Hellooooo!"
// Error: Type 'string' is not assignable to type 'Student'
```

## Classes e Interfaces

As Interfaces no TypeScript são uma forma poderosa de definir a estrutura de tipos em um código. Elas descrevem a forma de um objeto, especificando quais propriedades e métodos ele deve ter.

Uma classe do TypeScript pode implementar uma `interface`, garantindo assim que todas as instâncias daquela classe tenham os mesmos atributos e métodos:

```ts
interface User {
  name: string;
  setName(name: string): void;
  getName(): string;
};

class Student implements User {
  this.name: string;

  constructor(name: string) {
  	this.name = name;
  };

  setName(name: string): void {
  	this.name = name;
  };

  getName(): string {
  	return this.name;
  };
};
```

Qualquer incompatibilidade será considerado um type error pelo verificador de tipos:

```ts
class Teacher implements User {
  this.name: string;

  constructor(name: string) {
  	this.name = name;
  };

  getName(): string {
  	return this.name;
  };

  // Error: Class 'Teacher' incorrectly implements interface 'User'
  //   Property 'setName' is missing in type 'Teacher'
  //   but required in type 'User'
}
```

> A implementação de uma interface é puramente uma verificação de segurança. Ela não copia nenhuma propriedade da interface na definição da classe para você.

Implementar uma interface indica nossa intenção para o verificador de tipos e gera type error na definição de classe em vez de posteriormente, quando instâncias da classe forem usadas.

## Implementação de múltiplas interface

Uma classe pode implementar diversas interfaces:

```ts
interface Graded {
  grades: number[];
}

interface Reporter {
  report: () => string;
}

class ReportCard implements Graded, Reporter {
  grades: number[];

  constructor(grades: number[]) {
    this.grades = grades;
  }

  report() {
    return this.grades.join(", ");
  }
}

class Empty implements Graded, Reporter {
  // Error: Class 'Empty' incorrectly implements interface 'Graded'
  //   Property 'grades' is missing in type 'Empty'
  //   but required in type 'Graded'
  // Error: Class 'Empty' incorrectly implements interface 'Reporter'
  //   Property 'report' is missing in type 'Empty'
  //   but required in type 'Reporter'
}
```

## Extensão de uma classe (Herança)

O TypeScript permite estender/herdar propriedades e métodos de uma classe. Assim, as propriedades e métodos declaradas em uma classe base (classe pai) estarão disponíveis na subclasse (na classe filha), também conhecida como classe derivada:

```ts
class User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  getName(): string {
    return this.name;
  }
}

class Teacher extends User {
  constructor(name: string) {
    super(name);
  }

  teach() {
    console.log("Teaching...");
  }
}

class Student extends User {
  constructor(name: string) {
    super(name);
  }

  learn() {
    console.log("Learning...");
  }
}

const teacher = new Teacher("Anne");
teacher.getName(); // Ok (defined on base class)
teacher.teach(); // Ok (defined on subclass)

teacher.other();
// Error: Property 'other' does not exist on type 'Teacher'
```

## Classes abstratas

Classes abstratas são uma característica importante da programação orientada a objetos. Elas são usadas para definir modelos ou esqueletos de classes que não podem ser instanciados diretamente, mas servem como base para outras classes que estendem delas.

Em TypeScript, você pode criar uma classe abstrata adicionando o modificador **`abstract`** à sua declaração. Uma classe abstrata pode conter métodos abstratos, que são métodos que não têm implementação na classe abstrata, mas devem ser implementados nas subclasses concretas.

```ts
abstract class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  abstract makeNoise(): void;
}

class Dog extends Animal {
  makeNoise(): void {
    console.log(`${this.name} bark.`);
  }
}

class Cat extends Animal {
  makeNoise(): void {
    console.log(`${this.name} meow.`);
  }
}

const myDog = new Dog("Rex");
const myCat = new Cat("Whiskers");

myDog.makeNoise(); // Output: Rex bark.
myCat.makeNoise(); // Output: Whiskers meow.

class Fish extends Animal {
  // Error: Nonabstract class 'Fish' does not implement
  // inherited abstract member 'makeNoise' from class 'Animal'
}
```

> Uma classe abstrata não pode ser instanciada diretamente, já que ela não tem definições de alguns métodos que sua implementação presume que existam. Apenas classes não abstratas (”concretas") podem ser instanciadas.

```ts
let animal: Animal;

animal = new Animal("Teddy");
// Error: Cannot create an instance of an abstract class.
```

## Visibilidade dos membros

JavaScript incluiu a possibilidade de iniciarmos o nome do membro de uma classe com `#` para marcá-lo como membro `privado` de uma classe.

> Os membros de classe privados só podem ser acessados pelas instâncias desta classe.

O próprio runtime do JavaScript impõe um alerta quando uma área do código fora da classe tenta acessar a propriedade ou método privado.

Também é possível utilizar recursos mais avançados de restrições de visibilidade:

```ts
public(padrão);
// Pode ser acessado por qualquer pessoa, em qualquer local

protected;
// Só pode ser acessado pela classe e suas subclasses.

private;
// Só pode ser acessado pela classe.
```

> Essas palavras chaves só existem dentro da tipagem do TypeScript. Uma vez que o código é compilado paera JavaScript, esses recursos são removidos do código final.

A visibilidade de membros com o uso do `#` também fica disponível no `runtime` (já que é um código nativo do JavaScript). O que não ocorre com os modificadores de visibilidade do TypeScript (`public`, `protected` e `private`). Só os campos declarados como privados com o uso de `#` são realmente privados no runtime JavaScript.
