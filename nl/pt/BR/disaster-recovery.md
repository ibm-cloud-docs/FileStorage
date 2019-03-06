---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Recuperação de desastre: failover com um volume primário inacessível
{: #dr-inaccessible}

No caso de uma falha catastrófica ou de um desastre que cause uma indisponibilidade no site primário, os clientes podem executar as ações a seguir para acessar rapidamente seus dados no site secundário.

## Failover com uma duplicata de um volume de réplica no site secundário

1. Efetue login no [console do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/){:new_window} e clique no ícone **Menu** na parte superior esquerda. Selecione **Infraestrutura clássica**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
2. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
3. Clique na réplica do compartilhamento de arquivo na lista para visualizar sua página **Detalhes**.
4. Na página **Detalhes**, role para baixo e selecione uma captura instantânea existente e, em seguida, clique em **Ações** > **Duplicar**.
5. Faça quaisquer atualizações necessárias na capacidade (para aumentar o tamanho) ou na IOPS para o novo volume.
6. É possível atualizar o espaço de captura instantânea para o novo volume, se necessário.
7. Clique em **Continuar** para fazer seu pedido para a duplicata.

Assim que o volume for criado, será possível anexá-lo a um host e executar operações de leitura/gravação nesse volume. Enquanto os dados estão sendo copiados do volume original para a duplicata, é possível ver um status na página de detalhes mostrando que a duplicação está em andamento. Quando o processo de duplicação for concluído, o novo volume se tornará independente do original e poderá ser gerenciado com capturas instantâneas e replicação normalmente.

## Failback para o site primário original

Se você desejar retornar a produção para o site primário original, as etapas a seguir deverão ser executadas.

1. Efetue login no [console do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/){:new_window} e clique no ícone **Menu** na parte superior esquerda. Selecione **Infraestrutura clássica**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
2. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
3. Clique no nome do LUN e crie um planejamento de captura instantânea (se ainda não existir).

   Para obter mais informações sobre planejamentos de captura instantânea, consulte [Gerenciando capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots#addschedule).
   {:tip}
4. Clique em **Réplica** e em **Comprar uma replicação**.
5. Selecione o planejamento de captura instantânea existente que você deseja que a replicação siga. A lista contém todos os planejamentos de captura instantânea ativos.
6. Clique em **Local** e selecione o data center que era o site de produção original.
7. Clique em **Continuar**.
8. Clique na caixa de seleção **Eu li o Contrato de prestação de serviços principal…** e clique em **Colocar ordem**.

Após a replicação ser concluída, será necessário criar um volume duplicado da nova réplica.
{:important}

1. Volte para **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique na réplica da LUN na lista para visualizar sua página **Detalhes**.
3. Na página **Detalhes**, role para baixo e selecione uma captura instantânea existente e, em seguida, clique em **Ações** > **Duplicar**.
4. Faça quaisquer atualizações necessárias na capacidade (para aumentar o tamanho) ou na IOPS para o novo volume.
5. Atualize o espaço de captura instantânea para o novo volume, se necessário.
6. Clique em **Continuar** para fazer seu pedido para a duplicata.

Quando o processo de duplicação estiver concluído, será possível cancelar a replicação e os volumes que foram usados para que os dados voltem para o site primário original. A duplicata torna-se o armazenamento primário, e a replicação para o site secundário original pode ser estabelecida novamente.
