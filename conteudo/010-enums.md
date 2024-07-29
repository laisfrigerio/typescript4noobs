# Enums

Utilizamos `enum` como um apelido, afim de tornar o formato mais amigável, , definindo constantes que podem ser usadas para melhorar a legibilidade do código.

## Enums Numéricos

Esses enums serão incrementados conforme a primeira definição, exemplo:

```ts
enum Direction {
  UP = 1,
  DOWN,
  LEFT,
  RIGHT,
}
```

Seguindo a ordem, DOWN == 2, RIGHT == 3, etc... Nesse caso poderíamos declarar apenas as constantes, pois os valores iniciais começam do 0:

```ts
enum Direction {
  UP,
  DOWN,
  LEFT,
  RIGHT,
}
```

## Enums String

Também é possível definir `enum` com strings. Diferente dos numéricos, não há uma ordem de incremento, logo, é necessário inicializar com um valor real, como:

```ts
enum Direction {
  UP = "UP",
  DOWN = "DOWN",
  LEFT = "LEFT",
  RIGHT = "RIGHT",
}
```
