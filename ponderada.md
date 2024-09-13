# Proposta de Aprendizagem Contínua no Projeto Claus

## Introdução

O projeto Claus visa centralizar e automatizar o monitoramento de normas regulatórias do mercado financeiro brasileiro, facilitando a gestão e análise dessas normas por meio de técnicas de Processamento de Linguagem Natural (PLN). Uma parte crítica desse processo é a capacidade do sistema de se adaptar a mudanças contínuas nos dados e feedback fornecido pelos usuários, para que o modelo mantenha sua relevância e acurácia ao longo do tempo.

Nesse contexto, um conceito importante é o feedback loop. Um feedback loop em aprendizado de máquina ocorre quando um sistema ajusta seu comportamento com base em interações contínuas com os usuários. Ou seja, à medida que os usuários fornecem feedback — como editar tags de normas no Claus —, o sistema utiliza essas alterações para melhorar o modelo de PLN, garantindo que ele permaneça relevante e preciso. Isso permite que o sistema aprenda com os usuários de maneira contínua, aumentando sua inteligência e eficiência ao longo do tempo (C3.ai, 2024).

Dessa forma, a solução para manter o Claus adaptável e eficiente é implementar um ciclo de aprendizagem contínua. Isso permite que o modelo de PLN seja atualizado periodicamente com base nas edições de tags realizadas pelos usuários, tornando-o mais preciso e alinhado com a inteligência de negócios dos usuários.

## Solução Proposta

Para implementar o aprendizado contínuo no Claus, o sistema será adaptado para re-treinar o modelo de PLN de forma periódica (uma vez por semana), com base nas edições de tags realizadas pelos usuários. Quando uma tag é alterada, essa modificação é armazenada no banco de dados principal da aplicação, o mesmo que armazena as normas e suas respectivas tags.

A solução inclui os seguintes passos:

1. **Edição de Tags**: Os usuários editam as tags diretamente na interface do sistema. A rota da **Main API** processa essas edições e atualiza o banco de dados da aplicação, armazenando as novas tags junto às normas relacionadas.

2. **Armazenamento de Edições**: As alterações feitas pelos usuários são salvas no banco de dados principal da aplicação. Esse banco armazena as normas, suas tags, e as mudanças feitas ao longo do tempo.

3. **Retreinamento Semanal**: Um script é executado semanalmente para re-treinar o modelo de PLN, utilizando tanto os dados originais quanto as novas tags modificadas pelos usuários ao longo da semana. O retreinamento permite que o modelo aprenda com as alterações, ajustando sua capacidade de predição para as novas normas.

4. **Deploy do Novo Modelo**: Após o retreinamento, o novo modelo é versionado e colocado em produção, substituindo o modelo anterior, para que o sistema utilize as regras de tagueamento atualizadas nas futuras classificações.

### Diagrama de Blocos

```markdown
+------------------------+                +-------------------+                +-----------------------+
|                        |                |                   |                |                       |
| Interface do Usuário   |  --- edita --> |    Main API        | -- armazena -->| Banco de Dados         |
|  (Front-end)           |                | (Rota de Edição)   |                | (Armazena normas e tags)|
+------------------------+                +-------------------+                +-----------------------+
                                                                           |
                                                                           v
+------------------------+                +-------------------+            +-----------------------+
|                        |                |                   |            |                       |
|  Main API              | --- semanal -->| Script de Retreinamento |-- treina -->| Modelo de PLN           |
| (Tarefa Agendada)      |                | (Tarefa Agendada)  |            | (Modelo de Tagueamento) |
+------------------------+                +-------------------+            +-----------------------+
                                                                           |
                                                                           v
                                                                  +-----------------------+
                                                                  |                       |
                                                                  |  Deploy do Modelo      |
                                                                  | (Versionamento e Deploy)|
                                                                  +-----------------------+

```

### Descrição dos Módulos

- **Interface do Usuário**: Os usuários visualizam e editam as tags de normas no sistema. As alterações feitas pelos usuários são enviadas para o backend pela **Main API**.

- **Main API**: A **Main API** é responsável por processar as edições de tags. Uma rota específica coleta as mudanças e as atualiza no banco de dados da aplicação.

- **Banco de Dados**: O banco de dados principal armazena as normas, suas tags e as edições realizadas pelos usuários. Ele serve como fonte de dados para o re-treinamento do modelo.

- **Script de Retreinamento**: Um script agendado é executado semanalmente, processando as edições de tags feitas pelos usuários e re-treinando o modelo de PLN com base nessas mudanças.

- **Modelo PLN**: O modelo de PLN, que é responsável pelo tagueamento automático das normas, é atualizado semanalmente com base nas edições feitas pelos usuários e passa a utilizar as novas regras de tagueamento nas previsões futuras.

- **Deploy do Modelo**: Após o retreinamento, o novo modelo é versionado e colocado em produção, substituindo o modelo anterior para garantir que o sistema utilize as regras de tagueamento atualizadas.

## Conclusão

A proposta de implementar um retreinamento periódico no projeto Claus é uma solução prática e viável para garantir que o modelo de Processamento de Linguagem Natural se adapte às mudanças contínuas nas tags editadas pelos usuários. A decisão de realizar o retreinamento semanalmente permite que o sistema se mantenha atualizado sem sobrecarregar os recursos computacionais com re-treinamentos constantes em tempo real por exemplo.

Essa proposta requer um esforço de certa forma moderado porém como estamos com o projeto já bem encaminhado, estamos sim considerando a implementação dessa proposta. Principalmente porque acreditamos que a qualidade do modelo de PLN é crucial para a eficácia do projeto, destacada inclusive como o principal foco pelo parceiro, e os benefícios de um modelo adaptável e preciso, na minha visão pelo menos, colocam essa implementação extra como uma prioridade entre o que podemos focar em trabalhar nessas últimas duas sprints.

Para essa implementação, precisamos criar o script de retreinamento, que deve ser capaz de processar as edições de tags e re-treinar o modelo de PLN com base nessas mudanças. Além disso, é necessário garantir que o novo modelo seja versionado e implantado corretamente, substituindo o modelo anterior para que o sistema utilize a versão atualizada nas futuras classificações.

## Referências Bibliográficas

C3.ai. Feedback Loop in Machine Learning. Disponível em: <https://c3.ai/glossary/features/feedback-loop/>. Acesso em: 13 set. 2024.
