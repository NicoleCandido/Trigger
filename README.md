<h1 align="center">Triggers no MySQL</h1>

<p align="center">
  <img src="https://img.shields.io/badge/MySQL-8a2be2?style=for-the-badge&logo=mysql&logoColor=white" />
  <img src="https://img.shields.io/badge/SQL-ff69b4?style=for-the-badge&logo=databricks&logoColor=white" />
  <img src="https://img.shields.io/badge/Status-Concluído-8a2be2?style=for-the-badge" />
</p>

<p align="center">
  Banco de dados acadêmico que usa <b>triggers</b> para automatizar ações:<br>
  matrícula automática ao cadastrar um aluno e auditoria de alterações.
</p>

---

## Estrutura

```
Trigger/
├── aulas_trigger.sql   # Script completo do banco
└── README.md
```

## Modelo de dados

| Tabela      | Campos principais                                                      |
| :---------- | :--------------------------------------------------------------------- |
| `Curso`     | `cod` (PK), `descricao`                                                 |
| `Aluno`     | `cod` (PK), `nome`, `curso` (FK), `status`                              |
| `Matricula` | `cod` (PK), `aluno_cod` (FK), `curso_cod` (FK), `data_matricula`        |
| `Auditoria` | `id` (PK), `aluno_cod`, `nome_antigo`, `nome_novo`, `acao`, `modificado` |

## Triggers

| Trigger                     | Quando dispara           | O que faz                                                    |
| :-------------------------- | :----------------------- | :----------------------------------------------------------- |
| `tg_matricula_automatica`   | `AFTER INSERT` em Aluno  | Cria a matrícula do aluno no curso escolhido                 |
| `tg_auditoria_update_aluno` | `AFTER UPDATE` em Aluno  | Se o nome mudou, salva o nome antigo e o novo na Auditoria   |

```
INSERT em Aluno  ──►  tg_matricula_automatica   ──►  nova linha em Matricula
UPDATE em Aluno  ──►  tg_auditoria_update_aluno ──►  nova linha em Auditoria
```

## O que o script pratica

- Criação de triggers com `DELIMITER`, `BEGIN ... END`
- Uso de `NEW` e `OLD` para acessar os valores antes e depois da alteração
- Condição dentro do trigger com `IF`
- Chaves estrangeiras e valores padrão (`DEFAULT CURRENT_TIMESTAMP`)
- `DROP TRIGGER IF EXISTS` para o script poder ser executado mais de uma vez
