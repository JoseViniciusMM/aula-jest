# Teste unitário
"São testes que verificam se uma parte específica do código, costumeiramente a nível de função, está funcionando corretamente.
Em um ambiente orientado a objetos é usualmente a nível de classes e a mínima unidade de testes inclui construtores e destrutores. O objetivo do teste unitário é assegurar que cada unidade está funcionando de acordo com sua especificação funcional.
Estes tipos de testes são frequentemente escritos por desenvolvedores quando trabalham no código, para assegurar que a função específica está executando como esperado. Uma função deve ter muitos testes para dar cobertura a todos os caminhos possíveis do seu código.
Sozinho, o teste unitário não pode verificar a funcionalidade de uma parte do software, mas em contrapartida é usado para assegurar que os blocos constituintes do software trabalham independentes dos demais."

# Pré Requisitos para Aula 

Node.js (Versão ^16.0.0 LTS)
https://nodejs.org/en/

Visual Studio Code (VSC) (Usaremos extensões do code para realizar o teste)
https://code.visualstudio.com/

-------------------------------------------------------------------------
Criando um novo projeto em nodejs
```
npm init -y 
```
**ESlint para Teste Estático**
Instalando o ESlint 

Instalar a extensão do ESlint

```      
npm install --save-dev eslint 
npx eslint --init
```

-------------------------------------------------------------------------
**Jest** 
Framework de testes muito utilizado com o JavaScript

# Instalando Jest na versão mais atual 
```
npm install --save-dev jest
```
O Jest usa "matchers" para que você possa testar valores de maneiras diferentes.

Exemplo de teste
```
test('dois mais dois é quatro', () => {
  expect(2 + 2).toBe(4);
});
```
Nesse código, expect(2 + 2) retorna um objeto de "expectativa".
O toBe é o matcher que utiliza Object.is para testar a igualdade exata. 

# Matchers comuns

## Comparação que independem do tipo:
* toBe = testa se o valor passado é idêntico ao esperado em valor e tipo. 
* toEqual = testa recursivamente cada campo de um objeto ou array.
Para cada matcher de comparação é possível usar o .not para fazer uma comparação oposta.

* toBeNull = testa se o resultado passado tem valor igual a null.
* toBeUndefined = testa se o resultado passado tem valor igual a undefined.
* toBeDefined = testa se o resultado passado não tem valor igual a undefined.

* toBeTruthy testa se o resultado passado tem valor que pode ser passado como true em um if.
* toBeFalsy testa se o resultado passado tem valor que pode ser passado como false em um if.

## Usados para fazer comparações numéricas:
* toBeGreaterThan = testa se o resultado passado é maior que o esperado
* toBeGreaterThanOrEqual = testa se o resultado passado é maior ou igual ao esperado
* toBeLessThan = testa se o resultado passado é menor que o esperado.
* toBeLessThanOrEqual = testa se o resultado passado é menor ou igual ao esperado.
Os matchers toBe e toEqual são usados para testar equidade numérica.


* toMatch = verificar se uma certa string está dentro de uma expressão maior.
* toContain = verificar se um array possui um elemento em específico.
* toThrow = testar se uma determinada função lança um erro quando é chamada


Agora vamos criar um arquivo chamado 1aula.spec.js
```
    const {it, expect, describe} = require ('@jest/globals');

    it('Deve retornar a soma de 1 + 1 com toBe', () => {
        expect(1 + 1).toBe(2);
    });
    
    it('Deve retornar a igualdade de um objeto com toEqual', () => {
        const obj1 = {one:1}
        obj1.two = 2;
        expect(obj1).toEqual({ one: 1, two:2 });
    });
    
    it('Deve retornar se a string e existe na palavra teste com toMach', () => {
    
        expect('teste').toMatch(/e/);
    });
    
    it('Deve testar afirmações do jest', () => {
      const number = 10;
  
      expect(number).toBeLessThan(11);
      expect(number).toBeLessThanOrEqual(10);
  
      expect(number).toBeCloseTo(10.001);
      expect(number).toBeCloseTo(9.996);
  
      expect(number).not.toBeNull();
  
      expect(number).toHaveProperty('toString');
    });
  
    it('Deve testar afirmações do jest com numero', () => {
      const number = 10;
  
      expect(number).toBe(10);
      expect(number).toEqual(10);
  
      expect(number).not.toBeFalsy();
      expect(number).toBeTruthy();
  
      expect(number).toBeGreaterThan(9);
      expect(number).toBeGreaterThanOrEqual(10);
    });
 
  
    it('Deve testar afirmações do jest com objetos', () => {
      const person = { name: 'Luiz', age: 30 };
      const anotherPerson = { ...person };
  
      expect(person).toEqual(anotherPerson);
      expect(person).toHaveProperty('age');
      expect(person).not.toHaveProperty('lastName');
      expect(person).toHaveProperty('age', 30);
      expect(person.name).toBe('Luiz');
    });

```

Atenção a estrutura para criação dos testes no jest
 - describe - Bloco de testes-  suite de testes
 - test ou it - Declara único teste - caso de teste 
 - expect - Asserções do resultado - validar resultado

```
describe('Bloco de testes ', () => {
    it('Caso de teste', () => {
      //Asserções que validam o comportamento
        expect(1 + 1).toBe(2);
    })
});
    
```
