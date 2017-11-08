# Controle-de-Patrimonio-RFID-QRcode
Este repositorio foi criado para auxiliar na elaboração de um sistema baseado em tecnologia Arduino para realizar o controle do patrimônio de uma empresa, registrando todos os equipamentos em um banco de dados.

  Iniciando com testes de Placa Arduino Uno + Modulo RFID RC522
    A conexão direta e simples, carregando código apenas para leitura de tag, não mostrou resposta no monitor serial da IDE. Depois de muita tentativa, uma checagem na continuidade dos jumpers mostrou que a ligação entre os pinos 11-MOSI e 12-MISO apresentava abertura, ou
seja, não havia conexão por defeito nos jumpers. Com isso o programa era executado na serial, o comando solicitando apresentação da tag
aparecia no monitor, mas não havia resposta quando a tag era apresentada.
  A substituição dos jumpers defeituosos resolveu o problema.
  
  Para a compreençao do funcionamento do sistema, elaborou-se o seguinte programa:
- RFIDPequenoEditado_Alterado.ino
Usei duas tags diferentes representando cada produto.
