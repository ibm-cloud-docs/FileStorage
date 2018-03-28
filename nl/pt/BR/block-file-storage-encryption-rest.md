---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Protegendo seus dados - Criptografia em repouso gerenciada por provedor 

O {{site.data.keyword.BluSoftlayer_full}} leva a necessidade de segurança a sério e entende a importância de ser capaz de criptografar dados para mantê-los seguros. Com a criptografia gerenciada por provedor, o {{site.data.keyword.filestorage_full}} provisionado com o Endurance ou Performance é criptografado por padrão sem nenhum custo adicional e nenhum impacto no desempenho.

O recurso de criptografia em repouso gerenciada por provedor usa os protocolos padrão de mercado a seguir:

* Criptografia AES-256 de padrão de mercado
* As chaves são gerenciadas internamente com o Key Management Improbability Protocol (KMIP) de padrão de mercado
* O armazenamento é validado pela Publicação do Federal Information Processing Standard (FIPS) 140-2 para conformidade com o Federal Information Security Management Act (FISMA), o Health Insurance Portability and Accountability Act (HIPAA), o Payment Card Industry (PCI), o Basileia II, o California Security Breach Information Act (SB 1386) e o EU Data Protection Directive 95/46/EC

## Criptografia em repouso para capturas instantâneas ou armazenamento replicado  

Todas as capturas instantâneas e réplicas de armazenamento de arquivo criptografado também são criptografadas por padrão. Esse recurso não pode ser desligado em uma base de volume.

## Provisionando armazenamento com criptografia

O recurso de criptografia em repouso gerenciada por provedor está disponível somente para o {{site.data.keyword.filestorage_short}} que é provisionado em data centers selecionados com nova disponibilidade de data center sendo incluída regularmente. Todo o armazenamento provisionado nesses data centers é provisionado automaticamente com criptografia para dados em repouso. Clique [aqui](new-ibm-block-and-file-storage-location-and-features.html) para ver a lista atual de data centers em que a criptografia do {{site.data.keyword.filestorage_short}} para dados em repouso está disponível.


Ao pedir o {{site.data.keyword.filestorage_short}}, selecione um data center indicado com um asterisco (`*`) e a mensagem indicando que a criptografia está disponível. Você verá um ícone de bloqueio à direita do campo LUN/Nome do volume indicando que ele está criptografado. Veja a Figura 1.

![O ícone de bloqueio indica que o LUN está criptografado](/images/encryptedstorage.png)
<caption>Figura 1. Exemplo do ícone de bloqueio indicando que o volume está criptografado.</caption>



**Observe** que o armazenamento não criptografado provisionado antes de um upgrade de data center **não** será criptografado automaticamente. Se você tiver armazenamento não criptografado em um data center submetido a upgrade, precisará criar um novo volume e executar uma migração de dados. O artigo a seguir pode fornecer orientação.

* [Migração de armazenamento de arquivo em data centers submetidos a upgrade](migrate-file-storage-encrypted-file-storage.html)
