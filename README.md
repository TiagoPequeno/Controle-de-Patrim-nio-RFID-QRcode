# Controle-de-Patrimonio-RFID-QRcode
Este repositorio foi criado para auxiliar na elaboração de um sistema baseado em tecnologia Arduino para realizar o controle do patrimônio de uma empresa, registrando todos os equipamentos em um banco de dados.

  Iniciando com testes de Placa Arduino Uno + Modulo RFID RC522
    A conexão direta e simples, carregando código apenas para leitura de tag, não mostrou resposta no monitor serial da IDE. Depois de muita tentativa, uma checagem na continuidade dos jumpers mostrou que a ligação entre os pinos 11-MOSI e 12-MISO apresentava abertura, ou
seja, não havia conexão por defeito nos jumpers. Com isso o programa era executado na serial, o comando solicitando apresentação da tag
aparecia no monitor, mas não havia resposta quando a tag era apresentada.
  A substituição dos jumpers defeituosos resolveu o problema.
  
  Para a compreençao do funcionamento do sistema, elaborou-se o seguinte programa:
 
//Programa : RFID - Controle de Acesso leitor RFID (adaptado do programa facilmente encontrado na internet)
//Autor : Tiago Pequeno
//O programa simula a leitura de 2 produtos diferentes que pertencem ao patrimonio da empresa. Os produtos estão previamente registrados
//e o sistema apenas mostrar as informações de acordo com a tag do produto apresentado
 
#include <SPI.h>
#include <MFRC522.h>
//#include <LiquidCrystal.h>
 
#define SS_PIN 10
#define RST_PIN 9
MFRC522 mfrc522(SS_PIN, RST_PIN);   // Create MFRC522 instance.
 
//LiquidCrystal lcd(6, 7, 5, 4, 3, 2); 
 
//char st[20];
 
void setup() 
{
  Serial.begin(9600);   // Inicia a serial
  SPI.begin();      // Inicia  SPI bus
  mfrc522.PCD_Init();   // Inicia MFRC522
  Serial.println("Apresente Equipamento para Registro...");
  Serial.println();
 // Define o número de colunas e linhas do LCD:  
 // lcd.begin(16, 2);  
 // mensageminicial();
}
 
void loop() 
{
  // Look for new cards
  if ( ! mfrc522.PICC_IsNewCardPresent()) 
  {
    return;
  }
  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) 
  {
    return;
  }
  // Mostra dados do produto e UID da tag na serial
  String produto1 = "Equipamento Produto Consignado\n Camera IP JFL Full HD \n O produto tem valor de R$ xxxx,xx agregado e encontra-se alugada ao cliente Cond. Resid. Acaua desde xx/xx/xxxx, pelo que consta no contrato numero 00000 \n";
  String produto2 = "Equipamento Produto Particular\n Notebook Acer \n O produto tem valor de R$ xxxx,xx agregado e encontra-se cedido aos funcionarios da APS Sistemas de Seguranca na sede da empresa desde xx/xx/xxxx \n";
  String conteudo = "";
  Serial.print("UID da tag :");
  byte letra;
  for (byte i = 0; i < mfrc522.uid.size; i++) 
  {
     Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
     Serial.print(mfrc522.uid.uidByte[i], HEX);
     conteudo.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
     conteudo.concat(String(mfrc522.uid.uidByte[i], HEX));
  }
  Serial.println();
  Serial.print("Mensagem : ");
  conteudo.toUpperCase();
  
  // Testa se o cartao1 foi lido
  if (conteudo.substring(1) == "38 79 1B 00") //UID 1 - Chaveiro
  {
     Serial.println (produto1);
 //   lcd.clear();
 //   lcd.setCursor(0,0);
 //   lcd.print("Ola PEQUENO !");
 //   lcd.setCursor(0,1);
 //   lcd.print("Acesso liberado!");
    delay(3000);
  mensageminicial();
  }
 
  if (conteudo.substring(1) == "13 50 B8 EC") //UID 2 - Cartao
  {
     Serial.println (produto2);
  //  lcd.clear();
  //  lcd.setCursor(0,0);
  //  lcd.print("Ola Visitante !");
  //  lcd.setCursor(0,1);
  //  lcd.print("Acesso Liberado !");
    delay(3000);
  mensageminicial();
  }
} 

void mensageminicial()
{
  Serial.println("Apresente Equipamento para Registro...");
  Serial.println();
  //lcd.clear();
  //lcd.print(" Aproxime o seu");  
  //lcd.setCursor(0,1);
  //lcd.print("cartao do leitor");  
}

Usei duas tags diferentes representando cada produto.
