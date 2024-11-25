# Trigger
Este projeto demonstra o uso de triggers no MySQL para automatizar ações em tabelas de um banco de dados acadêmico.
AulasTrigger.sql
CREATE SCHEMA IF NOT EXISTS AulaTrigger DEFAULT CHARACTER SET utf8;
use Aulatrigger;

CREATE TABLE IF NOT EXISTS Alunos (
cod int not null auto_increment,
nome VARCHAR(100) not null,
curso int not null,
status int not null,
primary key (cod)
) ENGINE = InnoDB;

select * from Alunos;

CREATE TABLE IF NOT EXISTS Curso (
cod int not null auto_increment,
descricao VARCHAR(200) not null,
primary key (cod)
) ENGINE = InnoDB;

select * from Curso;

CREATE TABLE IF NOT EXISTS Matricula (
cod int not null auto_increment,
Aluno_cod int not null,
Curso_cod int not null,
primary key (cod)
) ENGINE = InnoDB;

select * from Matricula;

 INSERT INTO Curso VALUES (null,"Ciências da Computação");
 INSERT INTO Curso VALUES (null,"Tecnologia em Analise e desenvolvimento de sistemas");
 INSERT INTO Curso VALUES (null,"Engenharia da Computação");
 INSERT INTO Curso VALUES (null,"Sistema de Informação");
 
 select * from Curso;
 
 set delimiter $$
 create trigger tg_Matricula
 after insert on Alunos
 for each row begin
 insert into Matricula values (null, new.cod, new.Curso);
 end;
 
set delimiter ;
insert into Alunos values (null,"Carla Soares",2,1);
insert into Alunos values (null,"Fabricio Dos Reis",4,1);
select * from Alunos;

show triggers;

CREATE TABLE IF NOT EXISTS Auditoria (
id int not null auto_increment,
cod int not null,
nome varchar(100) not null,
modificado datetime default null,
acao varchar(50) default null,
primary key (id)
) ENGINE = InnoDB;

select * from Auditoria;

delimiter $$
create trigger antesDeUpdate_Alunos
before update on Alunos
for each row
begin 
insert into Auditoria
set acao = 'update',
cod = OLD.cod,
nome = OLD.nome,
modificado = now();
end$$
delimiter ; 

update Alunos set nome = "Carolina Freire" where cod = 1;
