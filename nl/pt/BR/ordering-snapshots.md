---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Solicitando capturas instantâneas

Para criar capturas instantâneas de seu volume de armazenamento, automatizadas ou manualmente, você precisa comprar espaço para retê-las. É possível comprar capacidade até sua quantia de volume de armazenamento (durante a compra de volume inicial ou posteriormente usando as etapas abaixo).

1. Acesse seu armazenamento por meio da guia **Armazenamento** > **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}
2. No quadro **Capturas instantâneas**, clique no link **Incluir espaço de
captura instantânea**.
3. No quadro **Capturas instantâneas**, clique em **Comprar espaço de
captura instantânea agora**.
3. Selecione a quantia de espaço que você precisa clicando no botão de opções próximo à quantia apropriada.
4. Clique em **Continuar**.
5. Insira qualquer Código promocional que você tiver e clique em **Recalcular**. Os
**Encargos para este pedido** e a **Revisão do pedido** terão valores
padrão.
6. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…** e clique em **Fazer pedido**. Seu espaço de captura instantânea será provisionado em alguns minutos.

## Como determinar a quantia de espaço de captura instantânea a ser solicitada

Infelizmente, não há uma melhor prática nem uma recomendação mais adequada com base no aplicativo. 
Genericamente falando, o espaço de captura instantânea é consumido por capturas instantâneas com base em duas
partes chave de informações:
- quantidade de mudanças que seu sistema de arquivos ativo tem e 
- por quanto tempo você planeja reter capturas instantâneas.  

Basicamente, a maneira de calcular a quantia de espaço necessária é **(Taxa de
mudança)** x **(número de horas/dias/semanas/meses retidos)**.  
**Nota**: a primeira captura instantânea consome uma quantia insignificante de
espaço, já que é apenas uma cópia de metadados (ponteiros) indicando os blocos do sistema de arquivos ativo. 

Um volume com muita mudança de dados (por exemplo, um banco de dados de taxa de mudança alta) e um
período longo de retenção de captura instantânea precisará de mais espaço para capturas instantâneas do que um
volume com mudança moderada (por exemplo, um armazenamento de dados da VM) e um planejamento de retenção de
captura instantânea mais moderado. 

No caso de um volume com 500 GB de dados reais, se você tirasse 12 capturas instantâneas por hora e
visualizasse 1% de mudança entre cada captura instantânea, você acabaria tendo (Taxa de mudança de 5 GB) x (12
capturas instantâneas por hora) = 60 GB para capturas instantâneas.

Por outro lado, se os 500 GB de dados reais, com 12 capturas instantâneas por hora, visualizassem
10% de mudança a cada hora, você acabaria tendo (Taxa de mudança de 50 GB) x (12 capturas instantâneas por
hora) = 600 GB.

Portanto, quando determinar a quantia de espaço de captura instantânea necessária, considere a taxa
de mudança cuidadosamente. Isso influencia enormemente a quantia de espaço de captura instantânea necessária. 
Enquanto o tamanho de um volume provavelmente significa uma quantia maior de mudança, um volume
de 500 GB com 5 GB de mudança e um volume de 10 TB com 5 GB de mudança resultariam no mesmo uso de
espaço de captura instantânea.

Além disso, para a maioria das cargas de trabalho, quanto maior for um volume, menos espaço precisará
ser reservado inicialmente para capturas instantâneas. Isso ocorre principalmente devido à eficiência dos dados
subjacentes da nossa plataforma, bem como devido à natureza de como as capturas instantâneas funcionam em
nosso ambiente.


