
Agora Crie um arquivo na raiz do projeto chamado index.js 
-> Dentro dele crie as funções de uma calculadora (soma, subtração, multiplicação e divisão)
-> Crie um teste para cada função no arquivo de Teste

index.js
```
const {it, expect, describe} = require ('@jest/globals');
const somaCalculadora = (a, b) => a + b;
const subCalculadora = (a, b) => a - b;
const mulCalculadora = (a, b) => a * b;
const divCalculadora = (a, b) => a / b;

module.exports = {
  somaCalculadora,
  subCalculadora,
  mulCalculadora,
  divCalculadora,
};
```
1aula.test.js
```
const {it, expect, describe} = require ('@jest/globals');
const { somaCalculadora, subCalculadora, mulCalculadora, divCalculadora } = require ('./index.js');

describe('Testes da Calculadora com valor inteiro', () => {
  it('Deve retornar a soma de dois valores', () => {
    const esperado = 30;
    const retornado = somaCalculadora(10, 20);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a subtração de dois valores', () => {
    const esperado = 10;
    const retornado = subCalculadora(20, 10);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a multiplicação de dois valores', () => {
    const esperado = 10;
    const retornado = mulCalculadora(1, 10);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a divisão de dois valores', () => {
    const esperado = 5;
    const retornado = divCalculadora(10, 2);
    expect(retornado).toBe(esperado);
  });
});

describe('Testes da Calculadora com valor flutuante', () => {
  it('Deve retornar a soma de dois valores flutuante', () => {
    const esperado = 26;
    const retornado = somaCalculadora(15.5, 10.5);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a subtração de dois valores flutuante', () => {
    const esperado = 10.399;
    const retornado = subCalculadora(20.7, 10.3);
    expect(retornado).toBeCloseTo(esperado);
  });
  it('Deve retornar a multiplicação de dois valores flutuante', () => {
    const esperado = 11.11;
    const retornado = mulCalculadora(1.1, 10.1);
    expect(retornado).toBeCloseTo(esperado);
  });
  it('Deve retornar a divisão de dois valores flutuante', () => {
    const esperado = 4.565;
    const retornado = divCalculadora(10.5, 2.3);
    expect(retornado).toBeCloseTo(esperado);
  });
});

describe('Testes da Calculadora com valor negativo', () => {
  it('Deve retornar a soma de dois valores negativo', () => {
    const esperado = -25;
    const retornado = somaCalculadora(-10, -15);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a subtração de dois valores negativo', () => {
    const esperado = -5;
    const retornado = subCalculadora(-15, -10);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a multiplicação de dois valores negativo', () => {
    const esperado = -10;
    const retornado = mulCalculadora(1, -10);
    expect(retornado).toBe(esperado);
  });
  it('Deve retornar a divisão de dois valores negativo', () => {
    const esperado = -5;
    const retornado = divCalculadora(-10, 2);
    expect(retornado).toBeCloseTo(esperado);
  });
});
```

Caso seja nescessario é possivel pular os testes adicionado a palavra skip após test ou it
```
it.skip('Deve retornar a soma de dois valores negativos', () => {
        const esperado = -25;
        const retornado = soma(-10,-15);
        expect(retornado).toBe(esperado);
    });
```
 
## Trantando exceções no teste toThrow

É possível que uma funcionalidade gere uma resposta inesperada, e o teste deve estar preparado para essa possibilidade. Por exemplo, em uma calculadora, se o divisor for igual a zero, não há cálculo possível — nesse caso, uma exceção é lançada e o teste deve prever esse comportamento.
index.js
```
const divCalculadora = (dividento,divisor) => {
    if(divisor == 0 ) {
        throw new Error ("Divisor Invalido" + divisor)
    }
    return dividento / divisor}

```
no arquivo de teste

```
  it('Deve retornar a exceção para divisor == 0 sendo exceção como resultado', () => {
        expect(() => divCalculadora(25,0)).toThrow();
    });
  it('Não deve gerar uma exceção ', () => {
        expect(() => divCalculadora(25,2)).not.toThrow();
    });
```

