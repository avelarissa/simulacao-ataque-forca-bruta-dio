# Desafio DIO de Segurança Cibernética: Ataques de Força Bruta com Kali Linux

## Descrição do Projeto

Este repositório documenta a execução do Desafio de Projeto da DIO em segurança cibernética, cuja meta foi simular e analisar cenários de ataques de força bruta (Brute Force) em um ambiente isolado. O laboratório foi composto por Kali Linux (atacante) e máquinas vulneráveis (Metasploitable2 e DVWA) em rede isolada (VirtualBox), onde foram executados testes contra serviços típicos (FTP, SMB e formulários web) utilizando ferramentas como Hydra e Medusa.

O trabalho inclui configuração do ambiente, wordlists e comandos utilizados, evidências das tentativas (capturas e logs), análise dos resultados e recomendações de mitigação práticas (bloqueio por tentativas, rate limiting, MFA, políticas de senha e monitoramento).

>⚠️ Aviso Ético-Legal: Todos os procedimentos, comandos, scripts e evidências deste repositório foram realizados em máquinas virtuais isoladas e destinam-se exclusivamente a fins educacionais, de pesquisa e auditoria interna. As máquinas e aplicações-alvo utilizadas (como Metasploitable2 e DVWA) são intencionalmente vulneráveis, permitindo a prática segura de técnicas de segurança ofensiva.
>
>O escopo das atividades foi estritamente limitado às máquinas virtuais descritas. Não houve varredura, exploração ou coleta de dados em sistemas ou redes de terceiros. A reprodução ou aplicação das técnicas aqui documentadas em sistemas sem autorização por escrito constitui ato ilícito, sujeito a responsabilização legal.

## Ferramentas e Tecnologias Utilizadas

A seguir estão descritas as principais tecnologias e ferramentas empregadas no laboratório e nos procedimentos experimentais.

| **Ferramenta / Tecnologia** | **Função principal**            | **Uso no laboratório** |
|-----------------------------|---------------------------------|------------------------|
| VirtualBox                  | Virtualização                   | Plataforma de virtualização utilizada para criar, importar e gerir as VMs do laboratório (Kali, Metasploitable2 e Logging VM). Suporta host-only/internal networks e snapshots para preservar estados. |
| Kali Linux                  | Plataforma atacante             | Sistema operacional atacante com suíte de ferramentas de pentest (hydra, medusa, nmap, etc.). |
| Metasploitable2             | Máquina alvo vulnerável         | VM propositalmente vulnerável utilizada como alvo para testes de serviços e exploração. |
| DVWA                        | Aplicação web vulnerável        | Aplicação alvo para testes de formulários, autenticação e ataques de força bruta. |
| Medusa                      | Brute force automatizado        | Ferramenta para ataques automatizados a serviços (ex.: FTP, SMB, SSH) durante os testes. |
| Hydra                       | Brute force & HTTP forms        | Ferramenta para força bruta em serviços e formulários web (HTTP form-post), usada como alternativa ao Medusa. |
| Nmap                        | Varredura e descoberta de rede  | Mapeamento de hosts, portas e versões de serviços (fingerprinting e scripts de descoberta). |
| enum4linux                  | Enumeração SMB                  | Coleta de informações SMB/Windows (shares, usuários, SIDs) para avaliação de superfície SMB. |
| smbclient                   | Cliente SMB                     | Acesso e testes de autenticação em shares SMB; verificação prática de permissões. |
| Wordlists (ex.: users.txt, passwords.txt) | Base para ataques | Listas de usuários e senhas usadas nas tentativas de força bruta. |


## Links oficiais para download

| Ferramenta / Tecnologia | Versão | Link                                                                                      |
| ----------------------- | -----: | ------------------------------------------------------------------------------------------|
| VirtualBox              |  7.2.2 | [virtualbox.org](https://www.virtualbox.org/)                                             |
| Kali Linux              | 2025.3 | [kali.org](https://www.kali.org/get-kali/#kali-platforms)                                 |
| Metasploitable 2        |     v2 | [Metasploitable2](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/) |

As demais ferramentas utilizadas neste laboratório são fornecidas por padrão nas imagens oficiais do Kali Linux. Se, por algum motivo, estiverem ausentes ou se preferir reinstalá‑las, é possível instalar/atualizar essas ferramentas diretamente pelo terminal do Kali com o gerenciador de pacotes ```apt```. 

```bash
sudo apt update && sudo apt install -y hydra medusa nmap enum4linux smbclient
```

Após a instalação, verifique as versões e a disponibilidade executando ```hydra -V```, ```medusa -V``` e ```nmap --version```.
