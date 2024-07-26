# Do JavaScript ao TypeScript

O TypeScript surgiu para superar alguns pontos fracos da linguagem de programação JavaScript.

Vamos ao seguinte exemplo:

```js
function somar(a, b) {
  return a + b;
}

let resultado = somar(5, "10"); // Sem erros durante a compilação, mas resultado inesperado durante a execução
console.log(resultado); // O resultado será "510" em vez de 15
```

É extremamente rápido começar a programar em JavaScript, mas pode ter um preço alto.

Neste exemplo, a função somar não tem tipagem explícita para seus parâmetros `a` e `b`, permitindo qualquer tipo de valor. Isso pode levar a bugs difíceis de rastrear, já que não há garantias de que os tipos de dados são os esperados.

No caso acima, a função é chamada com um número e uma string, resultando em uma concatenação de strings em vez de uma adição numérica. Como o JavaScript é uma linguagem de programaçãp dinamicamente tipada - que executa o código sem antes verificar se ele pode quebrar -, pode levar a muitos problemas em ambiente de produção.

Veja como esse mesmo código seria escrito em TypeScript:

```ts
const somar = (a: number, b: number): number => {
  return a + b;
};

const resultado: number = somar(5, "10"); // Erro de compilação: Argumento do tipo 'string' não pode ser atribuído ao parâmetro do tipo 'number'
console.log(resultado); // Não será possível chegar nesta linha devido ao erro de compilação
```

Aqui, a função somar é explicitamente tipada para aceitar apenas parâmetros do tipo number. Se você tentar chamar a função com uma string, o TypeScript emitirá um erro de compilação, impedindo a geração do código JavaScript. Isso ajuda a detectar e corrigir erros antes mesmo de executar o código.

## TypeScript

TypeScript é descrito como um “superconjunto (superset) do JavaScript” ou como uma “JavaScript tipado”.

O TypeScript é:

- **Uma linguagem de programação**: Uma linguagem que incluí toda a sintaxe existente do JavaScript, mais uma nova sintaxe específica do TypeScript para definição e uso de tipos.

- **Um verificador de tipos**: Um programa que acessa um conjunto de arquivos escritos em JavaScript e/ou TypeScript, desenvolve um conhecimento de todas as estruturas (variáveis, funções…) criadas e nos avisa se ele considera que algo foi definido incorretamente.

- **Um compilador**: Um programa que executa o verificador de tipos, relata qualquer problema e então retorna o código JavaScript equivalente.

## Principais benefícios

- **Detecção de erros durante a compilação**: O TypeScript fornece um sistema de tipos que ajuda a identificar possíveis erros antes mesmo de executar o código. Isso melhora a robustez e a confiabilidade do código.

- **Melhor documentação do código**: As tipagens em TypeScript servem como documentação integrada, facilitando a compreensão do código para outros desenvolvedores.

- **Autocompletar e sugestões**: Com tipagens adequadas, as IDEs podem fornecer autocompletar e sugestões mais precisas, aumentando a produtividade do desenvolvedor.

- **Refatoração segura**: O TypeScript facilita a refatoração do código com mais segurança, pois as mudanças são refletidas em todo o código devido às informações de tipo.
