# 🔐 Auditoria de Segurança: Ataques de Força Bruta com Kali Linux e Medusa

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como parte de um desafio prático da [Digital Innovation One (DIO)](https://www.dio.me/), com o objetivo de estudar ataques de força bruta (*Brute Force*) e técnicas de auditoria de segurança em ambientes controlados.

A proposta consiste em utilizar o Kali Linux e a ferramenta Medusa para compreender como tentativas repetidas de autenticação podem comprometer serviços e aplicações, além de identificar medidas que ajudam a prevenir esse tipo de ameaça.

Os testes propostos envolvem serviços FTP, formulários web utilizando DVWA e autenticação SMB, em máquinas virtuais isoladas.

> ⚠️ **Aviso de segurança:** Todos os experimentos devem ser realizados exclusivamente em ambientes próprios, isolados e autorizados. Não utilize as técnicas apresentadas contra sistemas, contas ou redes de terceiros.

## 🎯 Objetivos

- Compreender o funcionamento de ataques de força bruta.
- Conhecer a ferramenta Medusa e suas aplicações em auditorias de segurança.
- Preparar um laboratório utilizando máquinas virtuais.
- Estudar os riscos associados a senhas fracas e reutilizadas.
- Analisar formas de identificar e prevenir tentativas indevidas de autenticação.
- Documentar os procedimentos e resultados obtidos.
- Desenvolver boas práticas de documentação técnica utilizando Git e GitHub.

## 🧰 Tecnologias e Ferramentas

| Ferramenta | Finalidade |
|---|---|
| Kali Linux | Sistema operacional utilizado no laboratório de segurança. |
| Medusa | Ferramenta de auditoria de autenticação compatível com diferentes protocolos. |
| Metasploitable 2 | Máquina virtual vulnerável para estudos de segurança. |
| DVWA | Aplicação web intencionalmente vulnerável para fins educacionais. |
| VirtualBox | Virtualização das máquinas utilizadas no laboratório. |
| Nmap | Descoberta de hosts e identificação de serviços de rede. |
| Git e GitHub | Versionamento e documentação do projeto. |

## 🖥️ Arquitetura do Laboratório

O laboratório foi planejado para utilizar máquinas virtuais separadas, com comunicação restrita à rede de testes.

- **Máquina atacante:** Kali Linux.
- **Máquina de testes:** Metasploitable 2.
- **Aplicação web:** DVWA, executada em um ambiente vulnerável controlado.
- **Virtualização:** Oracle VirtualBox.
- **Rede:** Host-only ou rede interna isolada, conforme a configuração adotada.

### Topologia proposta

```text
        LABORATÓRIO ISOLADO
        VirtualBox
             |
       Rede de testes
             |
      +------+------+
      |             |
  Kali Linux    Metasploitable 2
  Auditoria      Serviços vulneráveis
                     |
                    FTP
                    SMB
                     |
              DVWA (ambiente web)
```

**Observação:** A topologia acima é ilustrativa. A localização do DVWA depende de onde a aplicação foi instalada. O modo Host-only permite comunicação entre o host e as VMs, enquanto a rede interna restringe a comunicação às máquinas virtuais conectadas à mesma rede.

## 🔎 Etapa 1 — Reconhecimento do Ambiente

Antes dos testes, é importante identificar os endereços IP das máquinas virtuais e verificar quais serviços estão disponíveis.

No Kali Linux, consulte as interfaces de rede:

```bash
ip addr
```

Para verificar a conectividade com a máquina de testes:

```bash
ping -c 4 IP_DA_MAQUINA
```

Substitua `IP_DA_MAQUINA` pelo endereço IP correspondente ao ambiente autorizado.

Para identificar serviços disponíveis no laboratório, pode-se utilizar o Nmap:

```bash
nmap -sV IP_DA_MAQUINA
```

O parâmetro `-sV` auxilia na identificação das versões dos serviços detectados.

**Resultado esperado:** obter informações sobre a conectividade e os serviços ativos, sem realizar testes contra endereços externos ou sistemas não autorizados.

## 🔑 Etapa 2 — Estudo de Ataques de Força Bruta

Um ataque de força bruta consiste em realizar várias tentativas de autenticação utilizando diferentes combinações de credenciais.

Essas tentativas podem explorar senhas curtas, previsíveis, reutilizadas ou presentes em listas de senhas conhecidas.

### 2.1 — FTP

O FTP é um protocolo tradicional de transferência de arquivos. Quando configurado sem proteção adequada, pode expor credenciais e permitir acessos indevidos.

Durante a auditoria, devem ser observados:

- Disponibilidade do serviço FTP.
- Configuração de autenticação.
- Existência de contas com senhas fracas.
- Registro de tentativas de autenticação.
- Restrições de acesso e permissões de arquivos.

**Medidas de prevenção:**

- Preferir SFTP ou outros mecanismos seguros de transferência.
- Desabilitar contas anônimas quando não forem necessárias.
- Aplicar senhas fortes e exclusivas.
- Restringir o acesso por firewall.
- Monitorar tentativas de login malsucedidas.

### 2.2 — Formulário Web com DVWA

O DVWA (*Damn Vulnerable Web Application*) é uma aplicação criada para permitir o estudo de vulnerabilidades web em um ambiente controlado.

O cenário de autenticação permite analisar como um formulário pode ser exposto a tentativas automatizadas de login.

Pontos de observação:

- Validação das credenciais.
- Mensagens de erro retornadas pela aplicação.
- Limites de tentativas de autenticação.
- Registro de eventos suspeitos.
- Comportamento dos mecanismos de proteção.

**Medidas de prevenção:**

- Implementar limitação de tentativas por conta e por origem.
- Adicionar atrasos progressivos quando apropriado.
- Utilizar autenticação multifator (MFA).
- Registrar e monitorar tentativas suspeitas.
- Evitar mensagens de erro que revelem informações desnecessárias.
- Proteger sessões e credenciais com HTTPS.

### 2.3 — SMB e Password Spraying

O SMB é um protocolo utilizado para compartilhamento de arquivos e outros recursos em redes.

O *password spraying* consiste em testar uma quantidade limitada de senhas comuns contra várias contas, tentando evitar os bloqueios que podem ocorrer quando muitas senhas são testadas em uma única conta.

Durante o estudo, devem ser considerados:

- Exposição do serviço SMB.
- Existência e gerenciamento de contas de usuários.
- Políticas de senha e bloqueio.
- Registros de falhas de autenticação.
- Permissões de compartilhamento.

**Medidas de prevenção:**

- Exigir senhas fortes e exclusivas.
- Aplicar autenticação multifator quando compatível.
- Desabilitar protocolos SMB antigos e inseguros.
- Restringir o acesso às portas SMB por meio de firewall.
- Monitorar falhas de autenticação distribuídas entre várias contas.
- Aplicar políticas de acesso com privilégio mínimo.

## 🧪 Etapa 3 — Wordlists e Testes Controlados

Wordlists são arquivos de texto que contêm palavras ou senhas utilizadas em testes de auditoria.

Para fins educacionais, podem ser criadas listas pequenas com credenciais fictícias, exclusivamente para contas de laboratório.

Exemplo de estrutura de arquivo:

**`wordlists/senhas-lab.txt`**

```text
SenhaFicticia123!
TesteLab456!
ExemploSeguro789!
```

Essas entradas são exemplos ilustrativos, não credenciais reais.

A ferramenta Medusa pode ser estudada para compreender seus módulos, parâmetros, protocolos suportados e formas de saída. Antes de executar qualquer teste automatizado, confirme o escopo, a autorização, o isolamento da rede e os limites de tentativas.

Para consultar a ajuda da ferramenta:

```bash
medusa -h
```

A documentação oficial do projeto pode ser consultada em:

http://www.foofus.net/jmk/medusa/medusa.html

Os resultados dos experimentos devem ser registrados de forma responsável, sem publicar senhas válidas, tokens, informações pessoais ou outros dados sensíveis.

## 📊 Etapa 4 — Registro dos Resultados

Os resultados devem refletir os testes realmente executados. A tabela abaixo serve como modelo para preenchimento após a realização dos experimentos.

| Cenário | Objetivo | Resultado |
|---|---|---|
| FTP | Analisar a autenticação do serviço. | A preencher após o teste. |
| DVWA | Estudar a proteção do formulário web. | A preencher após o teste. |
| SMB | Avaliar políticas de autenticação e registros. | A preencher após o teste. |
| Mitigações | Identificar melhorias de segurança. | A preencher após a análise. |

### Evidências

As capturas de tela podem demonstrar:

- Configuração da rede virtual.
- Endereços IP das máquinas de laboratório.
- Identificação dos serviços disponíveis.
- Configuração do DVWA.
- Registros de autenticação e mensagens de erro.
- Medidas de mitigação implementadas.

As imagens devem ser armazenadas na pasta `images/`, evitando dados sensíveis.

## 🛡️ Etapa 5 — Recomendações Gerais de Segurança

A prevenção contra ataques de força bruta exige a combinação de controles técnicos e boas práticas de gestão de identidades.

As principais recomendações são:

1. **Senhas fortes:** utilizar senhas longas, exclusivas e difíceis de adivinhar.
2. **MFA:** adicionar uma segunda etapa de verificação sempre que possível.
3. **Rate limiting:** limitar a frequência de tentativas de autenticação.
4. **Monitoramento:** registrar falhas e identificar padrões anormais.
5. **Bloqueios inteligentes:** aplicar controles contra tentativas repetidas sem facilitar ataques de negação de serviço.
6. **Firewall:** limitar os serviços às redes e aos dispositivos que realmente precisam acessá-los.
7. **Atualizações:** manter sistemas operacionais e serviços atualizados.
8. **Privilégio mínimo:** conceder apenas as permissões necessárias para cada conta.
9. **Proteção de credenciais:** não armazenar ou publicar senhas em texto simples.
10. **Auditorias periódicas:** revisar as configurações e testar os controles de segurança regularmente.

## 📁 Estrutura do Repositório

```text
auditoria-brute-force/
├── README.md
├── wordlists/
│   └── senhas-lab.txt
├── images/
│   ├── laboratorio.png
│   ├── reconhecimento.png
│   └── mitigacoes.png
└── docs/
    └── resultados.md
```

Os nomes dos arquivos são sugestões. Inclua apenas os arquivos e as evidências que realmente existirem no seu projeto.

## 📚 Referências

- [Kali Linux — Site oficial](https://www.kali.org/)
- [Medusa — Documentação](http://www.foofus.net/jmk/medusa/medusa.html)
- [DVWA — Damn Vulnerable Web Application](http://www.dvwa.co.uk/)
- [Nmap — Documentação oficial](https://nmap.org/book/)
- [Oracle VirtualBox](https://www.virtualbox.org/)
- [OWASP — Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [Microsoft Learn — SMB Security](https://learn.microsoft.com/en-us/windows-server/storage/file-server/smb-security)

## 💡 Aprendizados

Este projeto permite compreender como mecanismos de autenticação podem ser explorados quando não existem controles adequados contra tentativas repetidas de acesso.

Também reforça a importância do isolamento de ambientes de teste, do monitoramento de eventos e da aplicação de medidas preventivas para reduzir os riscos de comprometimento de contas e serviços.

A atividade contribui para o desenvolvimento de conhecimentos em segurança de redes, auditoria de serviços, análise de vulnerabilidades e documentação técnica.

## 👨‍💻 Autor

**Moisés Pierre Hanisch**

Projeto desenvolvido para fins educacionais como parte do desafio da Digital Innovation One (DIO).

[GitHub](https://github.com/Moises-hansich)

---

**Aviso:** Este repositório possui finalidade exclusivamente educacional. Qualquer teste de segurança deve ser realizado somente em sistemas próprios ou com autorização expressa dos responsáveis.
