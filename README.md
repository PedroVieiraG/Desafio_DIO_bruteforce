# Desafio_DIO_bruteforce
Este repositório contém a documentação e os artefatos do desafio prático realizado para a DIO (Digital Innovation One). O objetivo foi simular um ataque de força bruta em um ambiente controlado, utilizando o Kali Linux como máquina atacante e o Metasploitable 2 como alvo vulnerável.
🚀 Cenário do Projeto

O projeto consistiu em realizar a enumeração de serviços e a quebra de credenciais via protocolo FTP, simulando uma falha comum de segurança: o uso de senhas fracas.
🛠️ Ferramentas Utilizadas

    Hypervisor: Oracle VirtualBox

    Atacante: Kali Linux (v2025.4)

    Alvo: Metasploitable 2 (IP: 192.168.56.101)

    Rede: Host-Only (Rede interna para segurança)

    Ferramentas de Auditoria: nmap, ping, medusa, ftp

📋 Passo a Passo da Execução
1. Conectividade e Reconhecimento

O primeiro passo foi validar a comunicação entre as máquinas e identificar as portas abertas no alvo.

    Ping Test: Verificação de que o alvo estava ativo na rede.

    Nmap Scan: Identificação dos serviços rodando.
    Bash

    nmap -sV -p 21,22,80,445,139 192.168.56.101

        Resultado: Identificamos o serviço vsftpd 2.3.4 rodando na porta 21 (FTP).

2. Ataque de Força Bruta (FTP)

Utilizamos a ferramenta Medusa para testar combinações de usuários e senhas contidas em wordlists simples.

    Comando Utilizado:
    Bash

    medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6

    Parâmetros:

        -h: Host alvo.

        -U / -P: Caminho para as wordlists de usuários e senhas.

        -M ftp: Módulo de protocolo utilizado.

        -t 6: Número de threads (tentativas simultâneas).

3. Validação do Acesso

Após o Medusa encontrar a credencial válida (msfadmin:msfadmin), realizamos o acesso manual para confirmar a invasão.
Bash

ftp 192.168.56.101
# Login: msfadmin
# Password: msfadmin

    Status: 230 Login successful. Sistema remoto identificado como UNIX.

🛡️ Medidas de Mitigação

Para evitar que ataques de brute force tenham sucesso, as seguintes práticas são recomendadas:

    Políticas de Senhas Fortes: Implementar requisitos de complexidade (mínimo de 12 caracteres, símbolos e números). A complexidade da senha aumenta o tempo de quebra exponencialmente segundo a fórmula:
    T=SLn​

    Onde L é o conjunto de caracteres, n o comprimento da senha e S a velocidade de teste do atacante.

    Bloqueio de Conta (Account Lockout): Bloquear temporariamente o acesso após 3 ou 5 tentativas falhas (ex: usando Fail2Ban).

    Autenticação de Dois Fatores (2FA): Mesmo com a senha correta, o atacante precisaria de um segundo fator físico ou digital.

    Desativar Serviços Desnecessários: Se o FTP não for essencial, o serviço deve ser desativado ou restringido por IP (ACLs).
    
