```
@startuml
left to right direction
skinparam packageStyle rectangle
skinparam shadowing false
skinparam usecase {
BorderColor #555555
BackgroundColor #F9F9F9
}

title Sistema de Matrículas da Universidade — Casos de Uso

actor "Usuário" as Usuario
actor "Aluno" as Aluno
actor "Professor" as Professor
actor "Secretaria" as Secretaria
actor "Sistema de Cobranças" as Billing

Usuario <|-- Aluno
Usuario <|-- Professor
Usuario <|-- Secretaria

rectangle "Sistema de Matrículas da Universidade" as Sistema {

(Efetuar Login) as UC_Login

(Gerenciar currículo\ndo semestre) as UC_Curriculo
(Manter disciplinas) as UC_Disc
(Manter professores) as UC_Prof
(Manter alunos) as UC_Alunos

(Definir período de\nmatrículas) as UC_Periodo
(Encerrar período de\nmatrículas) as UC_EncerrarPeriodo
(Consolidar oferta de\ndisciplinas do semestre) as UC_Consolidar
(Ativar disciplina\n>= 3 alunos) as UC_Ativar
(Cancelar disciplina\n< 3 alunos) as UC_CancelarDisc

(Inscrever-se no semestre) as UC_Inscrever
(Matricular-se em disciplina) as UC_Matricular
(Cancelar matrícula) as UC_CancelarMat

(Validar regras de\nmatrícula) as UC_Validar
(Controlar vagas\nda disciplina) as UC_Vagas
(Encerrar matrículas\nda disciplina) as UC_EncerrarMatDisc

(Consultar alunos\npor disciplina) as UC_Consultar

(Notificar Sistema de\nCobranças) as UC_Notificar
}

' Associações principais
Usuario --> UC_Login

Secretaria --> UC_Curriculo
Secretaria --> UC_Periodo
Secretaria --> UC_EncerrarPeriodo

Aluno --> UC_Inscrever
Aluno --> UC_Matricular
Aluno --> UC_CancelarMat

Professor --> UC_Consultar

' Sistema externo
UC_Notificar --> Billing

' Decomposição / includes
UC_Curriculo --> UC_Disc : <<include>>
UC_Curriculo --> UC_Prof : <<include>>
UC_Curriculo --> UC_Alunos : <<include>>

UC_EncerrarPeriodo --> UC_Consolidar : <<include>>

UC_Inscrever --> UC_Matricular : <<include>>
UC_Inscrever --> UC_Notificar : <<include>>

UC_Matricular --> UC_Validar : <<include>>
UC_Matricular --> UC_Vagas : <<include>>

' Regras de negócio por extend
UC_EncerrarMatDisc ..> UC_Matricular : <<extend>>\n[quando vagas = 60]
UC_Ativar ..> UC_Consolidar : <<extend>>\n[inscritos >= 3]
UC_CancelarDisc ..> UC_Consolidar : <<extend>>\n[inscritos < 3]

' Observações / notas
note bottom of UC_Login
Pré-requisito para acessar os demais casos de uso.
Todos os usuários possuem senha para validação de login.
end note

note right of UC_Matricular
Aluno pode selecionar até:
- 4 disciplinas obrigatórias (1ª opção)
- 2 optativas (alternativas)
A matrícula ocorre apenas dentro do período definido.
end note

note right of UC_Vagas
Vagas por disciplina: 0..60
Ao atingir 60, novas matrículas são encerradas.
end note

note right of UC_Consolidar
Ao final do período de matrículas:
- Disciplinas com >= 3 alunos: ativadas
- Disciplinas com < 3 alunos: canceladas
end note
@enduml
```