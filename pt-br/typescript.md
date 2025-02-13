---
contributors:
    - ["Philippe Vlérick", "https://github.com/pvlerick"]
translators:
  - ["Gabriel Gomes", "https://github.com/gabrielgomesferraz"]
---

TypeScript é uma linguagem que visa facilitar o desenvolvimento de aplicações em grande escala escritos em JavaScript.
TypeScript acrescenta conceitos comuns como classes, módulos, interfaces, genéricos e (opcional) tipagem estática para JavaScript.
É um super conjunto de JavaScript: todo o código JavaScript é TypeScript válido então ele pode ser adicionado diretamente a qualquer projeto. O compilador emite TypeScript JavaScript.

Este artigo irá se concentrar apenas na sintaxe extra do TypeScript, ao contrário de [JavaScript](../javascript/).

Para testar o compilador TypeScript, vá para o [Playground](https://www.typescriptlang.org/Playground), onde você vai ser capaz de escrever código, ter auto conclusão e ver diretamente o JavaScript emitido.

```ts
// Existem 3 tipos básicos no TypeScript
var isDone: boolean = false;
var lines: number = 42;
var name: string = "Anders";

// Quando é impossível saber, há o tipo "Qualquer"
var notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // Ok, definitivamente um boolean

// Para coleções, não são matrizes e matrizes genéricas digitado
var list: number[] = [1, 2, 3];
// Como alternativa, usando o tipo de matriz genérica
var list: Array<number> = [1, 2, 3];

// Para enumerações:
enum Color {Red, Green, Blue};
var c: Color = Color.Green;

// Por último, "vazio" é utilizado no caso especial de uma função que não retorna nada
function bigHorribleAlert(): void {
  alert("I'm a little annoying box!");
}

// Funções são cidadãos de primeira classe, apoiar a sintaxe lambda "seta gordura" e
// Tipo de uso inferência

// A seguir são equivalentes, a mesma assinatura será inferida pelo
// Compilador, e o mesmo JavaScript será emitido
var f1 = function(i: number): number { return i * i; }
// Tipo de retorno inferido
var f2 = function(i: number) { return i * i; }
var f3 = (i: number): number => { return i * i; }
// Tipo de retorno inferido
var f4 = (i: number) => { return i * i; }
// Tipo de retorno inferido, one-liner significa nenhuma palavra-chave retorno necessário
var f5 = (i: number) =>  i * i;

// Interfaces são estruturais, qualquer coisa que tenha as propriedades é compatível com
// A interface
interface Person {
  name: string;
  // Propriedades opcionais, marcado com um "?"
  age?: number;
  // E de funções curso
  move(): void;
}

// Objeto que implementa a Interface "Pessoa"
// Pode ser tratado como uma pessoa desde que tem o nome e mover propriedades
var p: Person = { name: "Bobby", move: () => {} };
// Os objetos que têm a propriedade opcional:
var validPerson: Person = { name: "Bobby", age: 42, move: () => {} };
// Não é uma pessoa porque a idade não é um número
var invalidPerson: Person = { name: "Bobby", age: true };

// Interfaces também pode descrever um tipo de função
interface SearchFunc {
  (source: string, subString: string): boolean;
}
// Somente tipos dos parâmetros são importantes, os nomes não são importantes.
var mySearch: SearchFunc;
mySearch = function(src: string, sub: string) {
  return src.search(sub) != -1;
}

// Classes - membros são públicos por padrão
class Point {
  // Propriedades
    x: number;

    // Construtor - as palavras-chave public/private nesse contexto irão gerar
    // o código clichê para a propriedade e a inicialização no
    // construtor.
    // Neste exemplo, "y" será definida como "X" é, mas com menos código
    // Os valores padrão também são suportados.

    constructor(x: number, public y: number = 0) {
        this.x = x;
    }

    // Funções
    dist() { return Math.sqrt(this.x * this.x + this.y * this.y); }

    // Membros Estáticos
    static origin = new Point(0, 0);
}

var p1 = new Point(10 ,20);
var p2 = new Point(25); //y será 0

// Herança
class Point3D extends Point {
    constructor(x: number, y: number, public z: number = 0) {
        super(x, y); // Chamada explícita para o construtor da super classe é obrigatória
    }

    // Sobrescrever
    dist() {
        var d = super.dist();
        return Math.sqrt(d * d + this.z * this.z);
    }
}

// Módulos, "." pode ser utilizado como separador de sub módulos
module Geometry {
  export class Square {
    constructor(public sideLength: number = 0) {
    }
    area() {
      return Math.pow(this.sideLength, 2);
    }
  }
}

var s1 = new Geometry.Square(5);

// Alias no local para fazer referência a um módulo
import G = Geometry;

var s2 = new G.Square(10);

// Genericos
// Classes
class Tuple<T1, T2> {
    constructor(public item1: T1, public item2: T2) {
    }
}

// Interfaces
interface Pair<T> {
    item1: T;
    item2: T;
}

// e funções
var pairToTuple = function<T>(p: Pair<T>) {
    return new Tuple(p.item1, p.item2);
};

var tuple = pairToTuple({ item1:"hello", item2:"world"});

// Incluindo referências a um arquivo de definição:
/// <reference path="jquery.d.ts" />
```

## Leitura adicional

* [TypeScript site oficial](https://www.typescriptlang.org/)
* [Código fonte no GitHub](https://github.com/microsoft/TypeScript)
