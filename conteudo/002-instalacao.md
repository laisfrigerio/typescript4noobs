# Instalação

Você pode executar o TypeScript em seu computador contanto que tenha o NodeJS instalado. Para verificar se você tem o node instalado em seu computador, execute o seguinte comando:

```sh
node -v
```

Para instalar a última versão do TypeScript globalmente, execute o comando a seguir:

```sh
npm install -g typescript
```

Agora você pode executar o TypeScript na linha de comando usando o comando `tsc` (**T**ype**S**cript **C**ompiler - Compilador TypeScript).

Faça o teste com a flag —version para verificar se ele está instalado corretamente:

```sh
tsc --version
```

O comando acima deve exibir algo como Version X.Y.Z:

```sh
tsc --version
Version 4.7.2
```

> **Transpilador** é uma ferramenta que lê o código-fonte escrito em uma linguagem e produz um código equivalente em outra linguagem, com o mesmo nível de abstração.

## Inicializando projeto com TypeScript

Uma vez que o TypeScript está instalado, é hora de executá-lo localmente. Crie uma pasta em algum local em seu computador e execute este comando para gerar um novo arquivo de configuração `tsconfig.json`:

```sh
tsc --init
```

O seguinte comando executa o verificador de tipos, relata qualquer problema e então retorna o código JavaScript equivalente:

```sh
tsc index.ts
```
