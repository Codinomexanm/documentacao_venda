# Diagrama de Classes UML - Sistema GCM

```mermaid
classDiagram
    class Paciente {
        -String cpf
        -String nome
        -String endereco
        -String cep
        -String complemento
        -String foto
        -String whatsapp
        -String dadosGerais
        -Date aniversario
        -Boolean vip
        -Boolean cadastroCompleto
        +cadastrar()
        +editar()
        +excluir()
        +completarCadastro()
        +obterDados()
    }

    class Agendamento {
        -Date data
        -Time hora
        -String tipo
        -String interesse
        -String status
        -Long id
        +agendar()
        +reagendar()
        +cancelar()
        +alterar()
        +verificarStatus()
    }

    class Profissional {
        <<abstract>>
        -String nome
        -String cor
        -String tipo
        -Long id
        +realizarAgendamento()
        +criarPlanoTratamento()
    }

    class Medica {
        -String especialidade
        +criarPlanoTratamento()
        +realizarConsulta()
    }

    class Esteticista {
        -String areaAtuacao
        +criarPlanoTratamento()
        +realizarProcedimento()
    }

    class PlanoTratamento {
        -String categoria
        -String procedimento
        -String detalhamento
        -Integer quantidade
        -String observacoes
        -Boolean status
        -Long id
        +criar()
        +salvar()
        +limpar()
        +marcarComoFeito()
    }

    class Orcamento {
        -Double valorIndicado
        -Double valorFechado
        -Double desconto
        -String situacao
        -Date data
        -Long id
        +criar()
        +fechar()
        +fecharParcialmente()
        +aplicarDesconto()
        +imprimir()
        +editar()
    }

    class Experiencia {
        -String descricao
        -Date data
        -String tipo
        -Boolean destacado
        -Long id
        +registrar()
        +editar()
        +excluir()
        +destacar()
    }

    class Usuario {
        -String nome
        -String tipo
        -String[] permissoes
        -Long id
        +login()
        +logout()
        +verificarPermissao()
        +registrarExperiencia()
    }

    class Vendedora {
        -String areaVendas
        +criarOrcamento()
        +fecharVenda()
        +editarOrcamento()
    }

    class Recepcao {
        +agendarPaciente()
        +reagendarPaciente()
        +consultarAgendamentos()
        +registrarLigacoes()
    }

    class Marketing {
        -String persona
        -String[] campanhas
        -Date[] aniversarios
        +definirPersona()
        +criarCampanha()
        +verificarAniversarios()
        +analisarDadosPacientes()
    }

    class CRMRecepcao {
        -Integer pacientesCancelados
        -Integer pacientesFaltosos
        -Double indiceSucesso
        -Integer tarefasAbertas
        +listarCancelados()
        +listarFaltosos()
        +reagendar()
        +enviarMensagem()
        +calcularIndiceSucesso()
    }

    class TermoConsentimento {
        -String conteudo
        -Date dataAssinatura
        -Boolean assinado
        +gerar()
        +assinar()
        +validar()
    }

    class AntesDepois {
        -String fotoAntes
        -String fotoDepois
        -Date dataAntes
        -Date dataDepois
        -Long id
        +adicionarFoto()
        +comparar()
    }

    %% Relacionamentos de Herança
    Usuario <|-- Vendedora
    Usuario <|-- Recepcao
    Profissional <|-- Medica
    Profissional <|-- Esteticista

    %% Relacionamentos Paciente
    Paciente "1" --> "*" Agendamento : possui
    Paciente "1" --> "*" Experiencia : possui
    Paciente "1" --> "0..1" PlanoTratamento : possui
    Paciente "1" --> "0..1" Orcamento : possui
    Paciente "1" --> "*" TermoConsentimento : possui
    Paciente "1" --> "*" AntesDepois : possui

    %% Relacionamentos Agendamento
    Agendamento "*" --> "1" Paciente : pertence
    Agendamento "*" --> "1" Profissional : realizadoPor
    Agendamento "*" --> "0..1" CRMRecepcao : gerenciadoPor

    %% Relacionamentos Plano de Tratamento
    PlanoTratamento "1" --> "1" Paciente : pertence
    PlanoTratamento "1" --> "1" Profissional : criadoPor
    PlanoTratamento "1" --> "0..1" Orcamento : gera

    %% Relacionamentos Orçamento
    Orcamento "1" --> "1" PlanoTratamento : baseadoEm
    Orcamento "*" --> "1" Vendedora : criadoPor

    %% Relacionamentos Experiência
    Experiencia "*" --> "1" Paciente : pertence
    Experiencia "*" --> "1" Usuario : registradaPor

    %% Relacionamentos Marketing
    Marketing "1" --> "*" Paciente : analisa

    %% Relacionamentos CRM Recepção
    CRMRecepcao "1" --> "*" Agendamento : gerencia
```

## Descrição das Classes

### Paciente
Representa os pacientes da clínica. Contém informações pessoais, de contato e de cadastro. Pode ter múltiplos agendamentos, experiências registradas, planos de tratamento e orçamentos.

### Agendamento
Representa os agendamentos de consultas, retornos ou procedimentos. Está vinculado a um paciente e a um profissional (médica ou esteticista).

### Profissional (Classe Abstrata)
Classe base para Médica e Esteticista. Define atributos e comportamentos comuns aos profissionais que atendem na clínica.

### Médica
Profissional médico que pode realizar consultas e criar planos de tratamento.

### Esteticista
Profissional esteticista que pode realizar procedimentos e criar planos de tratamento.

### Plano de Tratamento
Plano criado por uma médica ou esteticista para um paciente, contendo procedimentos, categorias e detalhamentos.

### Orçamento
Orçamento baseado em um plano de tratamento, criado por uma vendedora. Pode estar aberto, fechado ou parcialmente fechado.

### Experiência
Registros de experiências do paciente na clínica, que podem ser destacadas para atenção especial.

### Usuário
Classe base para todos os usuários do sistema (recepção, médicas, esteticistas, vendedoras, marketing).

### Vendedora
Usuário especializado em criar e gerenciar orçamentos.

### Recepção
Usuário responsável pelo agendamento e reagendamento de pacientes.

### Marketing
Módulo responsável por análise de dados dos pacientes, definição de personas e campanhas.

### CRM Recepção
Sistema de gestão de relacionamento para acompanhar pacientes que cancelaram ou faltaram, com funcionalidades de reagendamento.

### Termo de Consentimento
Termos de consentimento assinados pelos pacientes.

### Antes e Depois
Fotos comparativas de antes e depois de tratamentos realizados pelos pacientes.

