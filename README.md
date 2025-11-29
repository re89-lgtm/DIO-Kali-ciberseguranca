# DIO-Kali-ciberseguranca
Projeto desafio para o curso de cibersegurança da DIO

Ambiente configurado utilizando duas máquinas virtuais, sendo o Metasploit2 no [link](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/) e o Kali linux no [link](https://www.kali.org/). Ambas configuradas com rede interna, host only para que as máquinas possam se enxergarem.
Foram feitos ataques de força bruta: no protocolo FTP e em formulários Web através do DVWA, ambos com criação de wordlists de usuários e senhas. Além disso foram feitos ataques em SMB com enumerção de usuários e Spraying de senhas de modo que este ataque tenha sido mais silencioso comparado com os primeiros.

Os comandos utilizados foram: 

FTP:
nmap -sV -p 21,22,80,445,139 para testar as portas
echo -e para a criação de possíveis nomes de usuários e senhas. Com isso foram gerados dois arquivos .txt
medusa -h para executar os ataques de força bruta utilizando os arquivos de texto gerados pelo comando echo.

DVWA:
aproveitamos as listas de usuários e senhas geradas anteriormente para esta etapa
medusa -h -M http \ -m PAGE:'/dvwa/login.php'\ -m FORM:'username=^USER^&password=^PASS^&Login=Login' \ -m'FAIL=Login failed' -t 6 para testar novamente os arquivos de usuários e senha desta vez no serviço Web.

SMB:
enum4linux -a | tee enum4_output.txt para testar todas as técnicas possíveis de enumeração na máquina alvo para obter dentre as informações disponíveis os nomes de usuário e grupos, além das políticas de senha. Também cria um arquivo .txt com todos os dados obtidos.
echo -e para criar uma wordlist com todos os usuários encontrados e também com possíveis senhas para utilizar o modo spraying. ESte modo é mais silencioso, pois ele não testa várias senhas consecutivamente para um mesmo usuário, mas ele testa uma senha para vários usuários, deste modo o sistema pode não perceber uma invasão.
medusa -h -M smbnt -t 2 -T 50 para testar nos vários usuários as possíveis senhas.


Nas pastas Images e Arquivos gerados estão os prints da tela com a evolução dos testes e os arquivos que o terminal do Kali gerou.

Estes ataques mostram que é relativamente fácil e rápido invadir uma máquina que esteja mal configurada e que os usuários utilizem senhas fracas.
Como mitigação para evitar estes problemas:
Quanto ao FTP recomenda-se desativar protocolos que não sejam necessários ou se forem necessários recomenda-se utilizar protocolos melhores como sftp ou scp que são considerados mais seguros do que o FTP.
Quanto ao Servidor WEb a recomendação é utilizar captcha, pois isso dificultaria a automação do ataque sendo necessário uma interação humana para ocorrer o processo de autenticação. Para tornar ainda mais seguro é recomendado que seja uma autenticação por 2 fatores o que mesmo que a senha seja descoberta o acesso não seria feito devido a segunda autenticação.
Quanto ao SMB, novamente é recomendado a autenticação por 2 fatores, além de bloqueio de IP's após múltiplas tentativas falhas de login.

E no geral a recomendação mais necessária é o uso de senhas fortes utlizando uma mistura de letras maiúsculas e minúsculas além de números e carcteres especiais. Isto tornaria todo o processo de ataque mais demorado e difícil. Tempo que levaria para o sistema e profissionais identificarem o ataque.
