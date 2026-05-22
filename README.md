# Diagrama C4 para arquitetura do sistema Goya

## Sobre

Desenvolvido para o módulo **Fundamentos de Design de Sistemas e Modelo C4** do curso *MIT em Arquitetura de Software - Infnet*

Aluno: Guilherme Marinho

Os diretórios `svg` e `png` contém, respectivamente, as exportações nos formatos '.svg' e '.png' do diagrama.

O arquivo `diagrama.c4.dsl` contém o mesmo código abaixo.

## Diagrama

> Basta copiar o código abaixo e colar em <a href="https://playground.structurizr.com/">https://playground.structurizr.com/</a>

```
workspace "Plataforma Goya" "Sistema de gestão operacional para boutique de treinamento físico" {

    !identifiers hierarchical

    model {

        ////////////////
        // Definições //
        ////////////////
        
        usuario = person "Usuário" "Aluno, instrutor ou administrador da plataforma."
        aluno = person "Aluno" "Cliente que agenda e consulta aulas."
        instrutor = person "Instrutor" "Responsável pelos treinamentos e gerenciamento da agenda."
        administrador = person "Administrador" "Responsável pela administração operacional da plataforma."
        
        goya = softwareSystem "Plataforma Goya" "Plataforma centralizada de gestão operacional e agendamentos." {

            app = container "Aplicação Web" "Aplicação web monolítica responsável pela interface, regras de negócio e integrações." "Java + Spring Boot + React" {

                identidade = component "Identidade e Acesso" "Responsável por autenticação, autorização, gerenciamento de usuários e permissões."

                agendamento = component "Agendamento" "Responsável por agendas, disponibilidade, agendamentos, remarcações e controle concorrente."

                administracao = component "Administração" "Responsável pelas configurações administrativas e unidades operacionais."

                notificacao = component "Notificação" "Responsável pelo envio de emails e notificações operacionais."
            }

            banco = container "PostgreSQL" "Banco de dados relacional da aplicação." "PostgreSQL" {
                tags "Database"
            }
        }
        
        emailSystem = softwareSystem "Serviço de Email" "Serviço externo utilizado para envio de notificações."
        
        /////////////////////
        // Relacionamentos //
        /////////////////////
        
        usuario -> goya "Acessa a plataforma"

        aluno -> goya.app "Agenda e consulta aulas"
        instrutor -> goya.app "Gerencia agenda e alunos"
        administrador -> goya.app "Administra a plataforma"

        goya.app -> goya.banco "Lê e escreve dados"

        goya.app -> emailSystem "Envia notificações e emails"

        goya.app.identidade -> goya.banco "Gerencia usuários e permissões"

        goya.app.agendamento -> goya.banco "Gerencia agendas e agendamentos"

        goya.app.administracao -> goya.banco "Gerencia configurações administrativas"

        goya.app.notificacao -> emailSystem "Envia emails e notificações"
    }

    views {

        systemContext goya "SystemContext" {
            include usuario
            include goya
            include emailSystem
            autolayout lr
        }

        container goya "Containers" {
            include *
            autolayout lr
        }

        component goya.app "ApplicationComponents" {

            include *
            autolayout tb
        }

        styles {

            element "Element" {
                color #ffffff
                stroke #0b4884
                background #1168bd
                strokeWidth 2
                shape roundedbox
            }

            element "Person" {
                background #08427b
                shape person
            }

            element "Software System" {
                background #1168bd
            }

            element "Container" {
                background #438dd5
            }

            element "Component" {
                background #85bbf0
                color #000000
            }

            element "Database" {
                shape cylinder
                background #2e7d32
            }

            relationship "Relationship" {
                thickness 2
                color #707070
            }
        }
    }

    configuration {
        scope softwaresystem
    }
}
```
