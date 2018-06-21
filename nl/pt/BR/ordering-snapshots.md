---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-18"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Solicitando capturas instantâneas

Para criar capturas instantâneas de seu volume de armazenamento, seja automaticamente ou manualmente, é necessário comprar espaço para mantê-las. É possível comprar capacidade até sua quantia de volume de armazenamento (durante a compra de volume inicial ou posteriormente usando estas etapas).

1. Acesse seu Armazenamento por meio da guia **Armazenamento** > **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}
2. No quadro **Capturas instantâneas**, clique em **Incluir espaço de captura instantânea**.
3. No quadro **Capturas instantâneas**, clique em **Comprar espaço de captura instantânea agora**.
3. Selecione a quantia de espaço que você precisa.
4. Clique em **Continuar**.
5. Insira qualquer Código promocional que você tiver e clique em **Recalcular**. **Encargos para este pedido** e **Revisão do pedido** têm valores padrão.
6. Clique na caixa de seleção **Eu li o contrato de prestação de serviços principal…** e clique em **Fazer pedido**. Seu espaço de captura instantânea será provisionado em alguns minutos.

## Como determinar quanto espaço de captura instantânea pedir

Infelizmente, não há uma melhor prática nem uma recomendação mais adequada com base no aplicativo. Genericamente falando, o espaço de captura instantânea é usado por capturas instantâneas com base em duas partes chave de informações:
- quantidade de mudanças que seu sistema de arquivos ativo tem e 
- por quanto tempo você planeja reter capturas instantâneas.  

Basicamente, a maneira de calcular a quantia de espaço necessária é **(Taxa de
mudança)** x **(número de horas/dias/semanas/meses retidos)**.  
**Nota**: a primeira captura instantânea usa uma quantia insignificante de espaço, já que é apenas uma cópia dos metadados (ponteiros) indicando os blocos do sistema de arquivos ativo. 

Um volume com muita mudança de dados (por exemplo, um banco de dados com alta taxa de mudança) e um período longo de retenção de captura instantânea precisará de mais espaço para capturas instantâneas do que um volume com taxa de mudança moderada (por exemplo, um armazenamento de dados da VM) e um planejamento de retenção de captura instantânea mais moderado. 

Se você fosse tirar 12 capturas instantâneas por hora de um volume com 500 GB de dados reais e visualizasse 1 por cento de mudança entre cada captura instantânea, acabaria usando 60 GB para capturas instantâneas.

*(5 GB de taxa de mudança) x (12 capturas instantâneas por hora) = (60 GB)*

Por outro lado, se esses 500 GB de dados reais com 12 capturas instantâneas por hora vissem 10 por cento de mudança a cada hora, você acabaria usando 600 GB.

*(50 GB de taxa de mudança) x (12 capturas instantâneas por hora) = 600 GB*

Portanto, quando determinar a quantia de espaço de captura instantânea necessária, considere a taxa
de mudança cuidadosamente. Isso influencia enormemente a quantia de espaço de captura instantânea necessária.  Embora o tamanho de um volume provavelmente signifique uma quantia maior de mudança, um volume de 500 GB com 5 GB de mudança e um volume de 10 TB com 5 GB de mudança resultariam no mesmo uso de espaço de captura instantânea.

Além disso, para a maioria das cargas de trabalho, quanto maior for um volume, menos espaço precisará
ser reservado inicialmente para capturas instantâneas.  Isso ocorre principalmente devido às eficiências dos dados subjacentes de nossa plataforma e à natureza de como as capturas instantâneas funcionam em nosso ambiente.


