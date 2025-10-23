# Desafio DIO de Segurança Cibernética: Ataques de Força Bruta com Kali Linux

## Descrição do Projeto
Este repositório documenta a execução do Desafio de Projeto da DIO em segurança cibernética, cuja meta foi simular e analisar cenários de ataques de força bruta (Brute Force) em um ambiente isolado. O laboratório foi composto por Kali Linux (atacante) e máquinas vulneráveis (Metasploitable2 e DVWA) em rede isolada (VirtualBox), onde foram executados testes contra serviços típicos (FTP, SMB e formulários web) utilizando ferramentas como Hydra e Medusa.

O trabalho inclui configuração do ambiente, wordlists e comandos utilizados, evidências das tentativas (capturas e logs), análise dos resultados e recomendações de mitigação práticas (bloqueio por tentativas, rate limiting, MFA, políticas de senha e monitoramento).

>⚠️ Aviso Ético-Legal: Todos os procedimentos, comandos, scripts e evidências deste repositório foram realizados em máquinas virtuais isoladas e destinam-se exclusivamente a fins educacionais, de pesquisa e auditoria interna. As máquinas e aplicações-alvo utilizadas (como Metasploitable2 e DVWA) são intencionalmente vulneráveis, permitindo a prática segura de técnicas de segurança ofensiva.
>
>O escopo das atividades foi estritamente limitado às máquinas virtuais descritas. Não houve varredura, exploração ou coleta de dados em sistemas ou redes de terceiros. A reprodução ou aplicação das técnicas aqui documentadas em sistemas sem autorização por escrito constitui ato ilícito, sujeito a responsabilização legal.
