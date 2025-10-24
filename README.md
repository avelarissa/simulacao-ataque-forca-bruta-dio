# Desafio DIO de Segurança Cibernética: Ataques de Força Bruta com Kali Linux

## Descrição do Projeto

Este repositório documenta o Desafio de Projeto da DIO em segurança cibernética. O objetivo foi simular e analisar cenários de ataques de força bruta em um ambiente isolado. O laboratório incluiu uma máquina atacante (Kali Linux) e alvos vulneráveis (Metasploitable2 e DVWA) em uma rede isolada via VirtualBox, onde foram realizados testes contra serviços típicos (FTP, SMB e formulários web) usando ferramentas de brute force como Hydra e Medusa.

O trabalho inclui configuração do ambiente, wordlists e comandos utilizados, evidências das tentativas (capturas e logs), análise dos resultados e recomendações de mitigação práticas (bloqueio por tentativas, rate limiting, MFA, políticas de senha e monitoramento).

>⚠️ Aviso Ético-Legal: Todos os procedimentos, comandos, scripts e evidências deste repositório foram realizados em máquinas virtuais isoladas e destinam-se exclusivamente a fins educacionais, de pesquisa e auditoria interna. As máquinas e aplicações-alvo utilizadas (como Metasploitable2 e DVWA) são intencionalmente vulneráveis, permitindo a prática segura de técnicas de segurança ofensiva.
>
>O escopo das atividades foi estritamente limitado às máquinas virtuais descritas. Não houve varredura, exploração ou coleta de dados em sistemas ou redes de terceiros. A reprodução ou aplicação das técnicas aqui documentadas em sistemas sem autorização por escrito constitui ato ilícito, sujeito a responsabilização legal.

## Ferramentas e Tecnologias Utilizadas

A seguir estão descritas as principais tecnologias e ferramentas empregadas no laboratório e nos procedimentos experimentais.

| **Ferramenta / Tecnologia** | **Função principal** | **Uso no laboratório** |
|-----------------------------|----------------------|------------------------|
| VirtualBox                  | Virtualização        | Plataforma de virtualização utilizada para criar, importar e gerir as VMs do laboratório (Kali, Metasploitable2 e Logging VM). Suporta host-only/internal networks e snapshots para preservar estados. |
| Kali Linux                  | Plataforma atacante  | Sistema operacional atacante com suíte de ferramentas de pentest (hydra, medusa, nmap, etc.). |
| Metasploitable2             | Máquina alvo vulnerável | VM propositalmente vulnerável utilizada como alvo para testes de serviços e exploração. |
| DVWA                        | Aplicação web vulnerável | Aplicação alvo para testes de formulários, autenticação e ataques de força bruta. |
| Medusa                      | Brute force automatizado | Ferramenta para ataques automatizados a serviços (ex.: FTP, SMB, SSH) durante os testes. |
| Hydra                       | Brute force & HTTP forms | Ferramenta para força bruta em serviços e formulários web (HTTP form-post), usada como alternativa ao Medusa. |
| Nmap                        | Varredura e descoberta de rede | Mapeamento de hosts, portas e versões de serviços (fingerprinting e scripts de descoberta). |
| enum4linux                  | Enumeração SMB       | Coleta de informações SMB/Windows (shares, usuários, SIDs) para avaliação de superfície SMB. |
| smbclient                   | Cliente SMB          | Acesso e testes de autenticação em shares SMB; verificação prática de permissões. |
| Wordlists (ex.: users.txt, passwords.txt) | Base para ataques | Listas de usuários e senhas usadas nas tentativas de força bruta. |


## Links oficiais para download

| Ferramenta / Tecnologia | Versão | Link                                                                          |
| ----------------------- | -----: | ----------------------------------------------------------------------------- |
| VirtualBox              |  7.2.2 | [virtualbox.org](https://www.virtualbox.org/)                                 |
| Kali Linux              | 2025.3 | [kali.org](https://www.kali.org/get-kali/#kali-platforms)                     |
| Metasploitable 2        |     v2 | [Metasploitable2](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/) |

As demais ferramentas utilizadas neste laboratório são fornecidas por padrão nas imagens oficiais do Kali Linux. Se, por algum motivo, estiverem ausentes ou se preferir reinstalá‑las, é possível instalar/atualizar essas ferramentas diretamente pelo terminal do Kali com o gerenciador de pacotes ```apt```. 

```bash
sudo apt update && sudo apt install -y hydra medusa nmap enum4linux smbclient
```

Após a instalação, verifique as versões e a disponibilidade executando:
```bash
hydra -V    # ou hydra --version
medusa -V   # ou medusa --version
nmap --version
```

## Configuração do Ambiente

A seguir estão as instruções reprodutíveis para criar um laboratório isolado com Kali Linux (atacante), Metasploitable2 (alvo) e uma rede Host-Only dedicada.

### A. Instalar o VirtualBox (GUI)
1. Acesse o site oficial do VirtualBox
2. Baixe o instalador adequado ao seu sistema (Windows / macOS / Linux).
3. Execute o instalador clicando duas vezes no arquivo baixado. Em seguida, siga as instruções do assistente e aceite a instalação dos drivers de rede e extensões quando solicitado.
4. Finalize e, se solicitado, reinicie o computador.
5. Abra o VirtualBox e confirme a interface.

### B. Criar a VM Kali (a partir da ISO)
1. No VirtualBox clique em New (Novo).
2. VM Name: Kali → ISO Image: selecione a ISO do Kali baixada → OS: Linux → OS Destribuition: Debian → OS Version: Debian (64-bit) → Next.

<div align="right">
  <details>
    <summary font-weight: bold;>
      [Configuração Kali Linux]
    </summary>
    <img src="images/Kali01.png" alt="Configuração Kali Linux" width="600">
  </details>
</div>

3. Para o uso neste laboratório, manteremos as especificações de hardware virtual recomendadas pelo próprio sistema.

<div align="right">
  <details>
    <summary font-weight: bold;>
      [Especificação de hardware virtual]
    </summary>
    <img src="images/Kali02.png" alt="Configuração Kali Linux" width="600">
  </details>
</div>

4. Na página seguinte, verifique se todas as especificações estão corretas e finalize a configuração da máquina virtual.
5. Selecione a VM criada e clique em Start (Iniciar) para iniciar a VM e executar o instalador do Kali.
6. Instale o Kali normalmente pelo instalador gráfico. Selecione o idioma e a região de preferência, defina um usuário e uma senha que serão utilizados posteriormente. Para este laboratório, não é necessário criar partições de armazenamento.

<div align="right">
  <details>
    <summary font-weight: bold;>
      [Instalação Kali Linux]
    </summary>
    <img src="images/Kali03.png" alt="Instalação Kali Linux" width="600">
  </details>
</div>

7. A instalação pode levar alguns minutos. Após concluída, o Kali estará pronto para uso.
