## Projeto Biblioteca
Vamos instalar o mongoose
```
npm install mongoose
```
Vamos instalar o mongo em memoria

Usar o MongoDB in-memory (mongodb-memory-server) em testes unitários permite simular um banco de dados real, mas rodando inteiramente na memória e de forma isolada, sem depender de uma instância externa. 
```
npm install --save-dev mongodb-memory-server
```
Agora crie a model Livro
src/models/Livro.js
```
import mongoose from 'mongoose';


const livroSchema = new mongoose.Schema({
  titulo: { type: String, required: true },
  autor: { type: String, required: true },
  anoPublicacao: { type: Number },
  genero: { type: String },
  disponivel: { type: Boolean, default: true }
}, {
  timestamps: true
});

module.exports = mongoose.model('Livro', livroSchema);

```
Crie o arquivo de teste
test/model/Livro.test.js
```
import mongoose from "mongoose";
import Livro from "../../src/models/Livro.js";
import { it, expect, describe, beforeAll, afterAll } from "@jest/globals";
import { MongoMemoryServer } from "mongodb-memory-server";


```
/*
| Funcionalidade          | Comportamento Esperado                                                          | Verificações                                                  | Critérios de Aceite                                                          |
| ----------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Cadastro de livro  {ok} | Um livro só pode ser cadastrado se possuir título e autor                       | Tentar salvar livro sem `titulo` ou `autor`                   | A operação deve falhar com erro de validação (`required`)                    |
| Cadastro válido {ok}    | Um livro com título, autor e demais campos válidos deve ser salvo com sucesso   | Inserir um livro com todos os campos preenchidos corretamente | O livro é salvo e retornado com `_id`, `createdAt` e `updatedAt`             |
| Valor padrão disponível {ok} | Ao cadastrar um livro sem informar `disponivel`, o valor padrão deve ser `true` | Cadastrar um livro sem o campo `disponivel`                   | O campo `disponivel` deve estar como `true` no documento salvo               |
| Registro de timestamps {ok} | O sistema deve registrar automaticamente as datas de criação e atualização      | Cadastrar um livro e verificar `createdAt` e `updatedAt`      | Os campos `createdAt` e `updatedAt` existem e são preenchidos corretamente   |
| Leitura de livros       | O sistema deve retornar todos os livros cadastrados                             | Fazer find() Verificar leitura dos dados inseridos            | A resposta contém um array com os livros cadastrados                         |
| Atualização de livro    | Deve ser possível atualizar informações de um livro válido                      | Fazer updateOne() / findByIdAndUpdate()                       | O livro deve refletir os dados alterados e o `updatedAt` deve ser atualizado |
| Remoção de livro        | Um livro existente pode ser removido do sistema                                 | Fazer deleteOne() / findByIdAndDelete()                       | O livro é removido e não aparece mais na listagem      
| Cadastro valido campos opcionais {ok} | O sistema deve permitir o cadastro de livros sem os campos opcionais  | Cadastrar um livro sem `anoPublicacao`, `genero` ou `disponivel` | O livro é salvo com os campos omitidos como `null` ou `undefined`            | 
*/
```

let mongoServer;
// Configuração do banco de dados em memória
beforeAll(async () => {
  mongoServer = await MongoMemoryServer.create();
  const uri = mongoServer.getUri();
  await mongoose.connect(uri);
    }
);
// Limpeza do banco de dados após os testes
afterAll(async () => {
  await mongoose.disconnect();
  await mongoServer.stop();
    }
);

// Testes para o modelo Livro
describe("Modelo Livro", () => {

  it("Deve criar um livro com os dados corretos- cadastro válido ", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: true,
    });

    const savedLivro = await livro.save();

    expect(savedLivro._id).toBeDefined();
    expect(savedLivro.titulo).toBe("O Senhor dos Anéis");
    expect(savedLivro.autor).toBe("J.R.R. Tolkien");
    expect(savedLivro.anoPublicacao).toBe(1954);
    expect(savedLivro.genero).toBe("Fantasia");
    expect(savedLivro.disponivel).toBe(true);
  });


  it("Deve retornar erro ao criar livro sem título - cadastro livro ", async () => {
    const livro = new Livro({
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: true,
    });
    let error;
    try {
      await livro.save();
    } catch (err) {
      error = err;
    }
   
    expect(error).toBeDefined();
    expect(error.name).toBe("ValidationError");
    expect(error.errors.titulo).toBeDefined();
    expect(error.errors.titulo.message).toBe("Path `titulo` is required.");
  });

  it("Deve retornar erro ao criar livro sem autor - cadastro livro", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: true,
    });
    let error;
    try {
      await livro.save();
    } catch (err) {
      error = err;
    }
    expect(error).toBeDefined();
    expect(error.name).toBe("ValidationError");
    expect(error.errors.autor).toBeDefined();
    expect(error.errors.autor.message).toBe("Path `autor` is required.");
  });


  it("Deve criar um livro com sem o campo 'disponivel' e confirmar que ele foi adicionado como default - Valor padrão disponível ", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia"
    });

    const savedLivro = await livro.save();

    expect(savedLivro.disponivel).toBe(true);
    expect(savedLivro.createdAt).toBeDefined();
    expect(savedLivro.updatedAt).toBeDefined();
  });

  it("Deve criar um livro com o campo 'disponivel' como false e confirmar campo disponivel  - Valor padrão disponível", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: false,
    });

    const savedLivro = await livro.save();

    expect(savedLivro.disponivel).toBe(false);
  });

  it("Deve criar um livro sem o campo 'anoPublicacao' - Cadastro valido campos opcionais", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      genero: "Fantasia",
      disponivel: true,
    });

    const savedLivro = await livro.save();

    expect(savedLivro.anoPublicacao).not.toBeDefined();
  });

  it("Deve criar um livro sem o campo'genero' - Cadastro valido campos opcionais", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      disponivel: true,
    });

    const savedLivro = await livro.save();

    expect(savedLivro.genero).not.toBeDefined;
  });
  
  it("Deve criar um livro com o campo 'disponivel' como null", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: null,
    });

    const savedLivro = await livro.save();

    expect(savedLivro.disponivel).toBeNull();
  });

  it("Deve criar um livro com o campo 'anoPublicacao' como undefined", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      genero: "Fantasia",
      disponivel: true,
    });

    const savedLivro = await livro.save();

    expect(savedLivro.anoPublicacao).toBeUndefined();
  });

  it("Deve criar um livro com o campo 'genero' como undefined", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      disponivel: true,
    });

    const savedLivro = await livro.save();

    expect(savedLivro.genero).toBeUndefined();
  });

  it("Deve listar todos os livros cadastrados - Leitura de livros", async () => {
    const livro1 = new Livro({
      titulo: "O Morro dos Ventos Uivantes",
      autor: "Emily Brontë",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: true,
    });

    const livro2 = new Livro({
      titulo: "Cem Anos de Solidão",
      autor: "Gabriel García Márquez",
      anoPublicacao: 1949,
      genero: "Ficção Científica",
      disponivel: true,
    });

    await livro1.save();
    await livro2.save();

    const livros = await Livro.find();
  
    expect(livros).toBeInstanceOf(Array);
    expect(livros.length).toBeGreaterThanOrEqual(2);
    expect(livros.map(l => l.titulo)).toEqual(
      expect.arrayContaining(["Cem Anos de Solidão", "O Morro dos Ventos Uivantes"])
    );
  });

  it("Deve atualizar um livro existente - Atualização de livro", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: true,
    });

    const savedLivro = await livro.save();

    const updatedLivro = await Livro.findByIdAndUpdate(
      savedLivro._id,
      { titulo: "O Hobbit" },
      { new: true }
    );

    expect(updatedLivro.titulo).toBe("O Hobbit");
    expect(updatedLivro.updatedAt).not.toEqual(savedLivro.updatedAt);
  });

  it("Deve remover um livro existente - Remoção de livro", async () => {
    const livro = new Livro({
      titulo: "O Senhor dos Anéis",
      autor: "J.R.R. Tolkien",
      anoPublicacao: 1954,
      genero: "Fantasia",
      disponivel: true,
    });

    const savedLivro = await livro.save();

    await Livro.findByIdAndDelete(savedLivro._id);

    const deletedLivro = await Livro.findById(savedLivro._id);

    expect(deletedLivro).toBeNull();
  });
  
  it("Deve retornar null ao tentar atualizar um livro inexistente - Atualização de livro", async () => {
    const idInexistente = "60d5f484f1c2b8a0b8c8e4e1"; 
  
    const resultado = await Livro.findByIdAndUpdate(
      idInexistente,
      { titulo: "O Hobbit" },
      { new: true } 
    );
  
    expect(resultado).toBeNull(); 
  });
  
  
  it("Deve retornar erro ao tentar remover um livro inexistente - Remoção de livro", async () => {
    const idInexistente = "60d5f484f1c2b8a0b8c8e4e1"; 

    const resultado = await Livro.findByIdAndDelete(idInexistente);
  
    expect(resultado).toBeNull();


  });

  it("Deve lançar erro ao tentar remover um livro com ID malformado - Remoção de livro", async () => {
    await expect(Livro.findByIdAndDelete("60d5f484f1")).rejects.toThrow();
  });


});
```