# Importação dos Modulos
**CommonJS** (CJS) e **ECMAScript Modules** (ESM)

## Diferença entre CommonJS e ECMAScript Modules (ESM)
1. O que são?
CommonJS (CJS) é o sistema de módulos tradicional do Node.js.

ECMAScript Modules (ESM) é o padrão moderno de módulos definido no ES6 (2015), adotado nativamente pelos navegadores e suportado pelo Node.js (a partir da versão 12 com suporte estável na 14+).

# Comparação entre CommonJS e ES6 Modules

| Característica          | CommonJS (CJS)                          | ES6 Modules (ESM)                     |
|-------------------------|----------------------------------------|---------------------------------------|
| **Sintaxe de Exportação** | `module.exports = ...` ou `exports.var = ...` | `export default ...` ou `export { var }` |
| **Sintaxe de Importação** | `const mod = require('modulo')`       | `import mod from 'modulo'`            |
| **Tipo de Carregamento**  | Síncrono                               | Assíncrono                            |
| **Local das Importações** | Em qualquer lugar do código           | Somente no topo do arquivo (estático) |
| **Suporte no Node.js**    | Suporte nativo desde sempre           | Necessita `.mjs` ou `"type": "module"` |
| **Suporte em Navegadores** | Não suportado nativamente             | Suporte nativo                       |
| **Exemplo Exportação**   | ```module.exports = { func }```       | ```export const func = () => {}```    |
| **Exemplo Importação**   | ```const { func } = require('./mod')```| ```import { func } from './mod'```    |
| **Cyclic Dependencies**  | Suportado com limitações              | Suportado melhor                      |
| **Extensões de Arquivo** | `.js` ou `.cjs`                       | `.js` (com flag) ou `.mjs`            |

# Quando Usar Cada Um

- **Prefira ES6 Modules** para:
  - Novos projetos
  - Aplicações frontend (React, Vue, etc.)
  - Quando precisar de tree-shaking

- **Use CommonJS** para:
  - Projetos Node.js legados
  - Pacotes que precisam de compatibilidade
  - Quando precisar de imports dinâmicos

3. Como isso afeta os testes?
Frameworks como Jest, por padrão, usam CommonJS, então você vê muitos require(...) nos testes.

Se o seu projeto usa ES6, é necessário configurar o Jest para entender a sintaxe import/export,para que possamos utilizar o padrão de importação ES6 podemos usar a versão experimental do Jest:

```
"scripts": {
    "test":"node --experimental-vm-modules node_modules/jest/bin/jest.js",
    "test:watch":"node --experimental-vm-modules node_modules/jest/bin/jest.js --watchAll",
    "test:coverage":"node --experimental-vm-modules node_modules/jest/bin/jest.js --watchAll --coverage"
  },

```
Mas, como queremos utilizar todas as funcionalidades presentes nas funções simuladas (Mocks) do Jest vamos precisar de um tradutor, compilador/transpilador de código neste caso o Babel. 

Instalação com suas dependecias:
```
npm install --save-dev @babel/core @babel/preset-env babel-jest
```
Adicione ao package.json:

```
{
  "type": "module",
}
```
Dentro de Scripts
```
  "scripts": {
    
    "test": "jest --coverage --detectOpenHandles",
  },
```

# Configuração Babel Básica

```
"devDependencies": {
    "@babel/core": "^7.29.0",
    "@babel/preset-env": "^7.29.0",
    "babel-jest": "^30.2.0",
    "jest": "^30.2.0"
  }, 
  "babel": {
    "presets": [
      [
        "@babel/preset-env",
        {
          "targets": {
            "node": "current"
          }
        }
      ]
    ]
  }
```

# Configuração Babel Básica e do Jest com arquivo de configuração

Abaixo de devDependencies depois das chaves "Avançado Explicarei mas a frente "
```
 "devDependencies": {
   
  },
  "babel": {
    "presets": [
      [
        "@babel/preset-env",
        {
          "targets": {
            "node": "current"
          }
        }
      ]
    ]
  },
  "jest": {
    "transform": {
      "^.+\\.js$": "babel-jest"
    },
    "testEnvironment": "node",
    "verbose": true,
    "transformIgnorePatterns": [
      "/node_modules/"
    ],
    "setupFilesAfterEnv": [
      "<rootDir>/jest.setup.js"
    ],
    "coveragePathIgnorePatterns": [
      "/node_modules/",
      "/utils/helpers/index.js",
      "/utils/logger.js"
    ]
  }
```
crie um arquivo na raiz do projeto com o nome ```babel.config.cjs```dentro dele deve conter a seguinte configuração:
```
module.exports = {
    presets: [
      ['@babel/preset-env', { targets: { node: 'current' } }]
    ]
  };

```
crie um arquivo na raiz do prijeto com o nome ```jest.setup.js``` dentro dele deve conter a seguinte configuração:
```
// Arquivo de configuração executado pelo Jest após a inicialização do ambiente de teste
// (configurado em setupFilesAfterEnv no jest.config.js)

// Executa antes de TODOS os testes do conjunto de testes (test suite)
beforeAll(() => {
    // Espiona e mocka a função console.error:
    // - Substitui a implementação original por uma função vazia
    // - Permite verificar se foi chamada (com jest.toHaveBeenCalled())
    jest.spyOn(console, 'error').mockImplementation(() => { });
    
    // Espiona e mocka a função console.log:
    // - Similar ao console.error, mas para logs comuns
    jest.spyOn(console, 'log').mockImplementation(() => { });
});

// Executa depois de TODOS os testes do conjunto de testes
afterAll(() => {
    // Restaura a implementação original do console.error se existir
    // (verificação necessária para evitar erros caso o mock não tenha sido criado)
    if (console.error.mockRestore) {
        console.error.mockRestore();
    }
    
    // Restaura a implementação original do console.log se existir
    if (console.log.mockRestore) {
        console.log.mockRestore();
    }
});
```

Apos as configurações temos que acertar o modulo de importação para ES6
Dentro de index.js vamos mudar a exportação
```
export {somaCalculadora, subCalculadora, mulCalculadora, divCalculadora};

```
Dentro de 1aula.tes.js vamos mudar a importação
```
import {it, expect, describe} from '@jest/globals';
import { somaCalculadora, subCalculadora, mulCalculadora, divCalculadora } from './index.js';
```

Agora vamos criar outro arquivo 2aula.spec.js


## Variaveis Globais 
beforAll, beforEach , afterAll, afterEach


Dentro do index.js vamos adicionar uma função de média
```
const media =(val1,val2,val3) => {return (val1 + val2 + val3)/3}
export {somaCalculadora, subCalculadora, mulCalculadora, divCalculadora, media};
```
Dentro do arquivo 2aula.spec.js
```
import {it, expect, describe} from '@jest/globals';

var val1
var val2 
var val3

beforeAll(()=> {
    console.log("Antes dos testes");
    val1= 4
    val2= 8
    val3= 7
});

afterEach(()=>{
    console.log("Depois de cada teste");
    val1= 3
    val2= 5
    val3= 4
});

afterAll (()=>{
    console.log("Após os teste");
    val1= null;
    val2= null;
    val3= null;
});

describe ('Checando média',() => {
  it('Deve retornar se a média das notas 4, 8, 7 é maior que 6 ', () => {
    console.log(val1+ "" + val2 + "" + val3);
    expect(media(val1,val2,val3)).toBeGreaterThanOrEqual(6);
  });
  it('Deve retornar se a média das notas 3, 5, 4 é menor que 6 ', () => {
    console.log(val1+ "" + val2 + "" + val3);
    expect(media(val1,val2,val3)).toBeLessThanOrEqual(6);
  });
  
});
```
