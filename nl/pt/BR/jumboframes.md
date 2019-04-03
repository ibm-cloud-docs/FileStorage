---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NSF, networking, jumbo frames

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Ativando Quadros gigantes no {{site.data.keyword.BluSoftlayer_notm}} para Windows e Linux
{: #jumboframes}

Um quadro gigante é um quadro Ethernet com uma carga útil maior que a unidade máxima de transmissão (MTU) padrão de 1.500 bytes. Os quadros gigantes são usados em redes locais que suportam pelo menos 1 Gbps e podem ser tão grandes quanto 9.000 bytes.

Os quadros gigantes precisam ser configurados da mesma maneira em todo o caminho de rede do dispositivo de origem <-> comutador <-> roteador <-> comutador <-> dispositivo de destino. Se a cadeia inteira não for configurada da mesma maneira, ela será padronizada para a configuração mais baixa ao longo da cadeia. O {{site.data.keyword.BluSoftlayer_full}} tem dispositivos de rede configurados para 9.000 atualmente. Todos os dispositivos do cliente precisam ser configurados com o mesmo valor 9.000.
{:important}

## Ativando Quadros Jumbo no Windows

1. Abra o **Centro de rede e compartilhamento**.
2. Clique em **Mudar as configurações do adaptador**.
3. Clique com o botão direito no NIC para o qual você deseja ativar quadros gigantes e selecione **Propriedades**.
4. Na guia **Rede**, clique em **Configurar** para o adaptador de rede.
5. Selecione a guia **Avançado**.
6. Selecione **Quadro gigante** e mude o valor de **disabled** para o valor desejado. O valor, como 9 kB ou 9.014 bytes, depende do NIC.
7. Clique em  ** OK **  em todas as janelas.

Quando você faz a mudança, a NIC perde a conectividade de rede por alguns segundos. Reinicie o dispositivo para confirmar que a mudança foi efetivada.
{:tip}


## Ativando Quadros Jumbo no Linux

1. Edite o arquivo de configuração de rede para a interface eth0.
   - Os usuários do CentOS, RHEL, Fedora Linux editam `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Os usuários do Debian e Ubuntu Linux editam `/etc/network/interfaces`.

2. Anexe a diretiva de configuração a seguir, que especifica o tamanho do quadro em bytes.
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian e Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Feche e salve o arquivo. Reinicie a Interface eth0.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   Essa ação causa uma breve perda de conectividade de rede.
