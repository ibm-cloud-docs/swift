---

copyright:
  years: 2018
lastupdated: "2018-08-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation du Stockage d'objets pour le contenu statique
{: #object}

Le Stockage d'objets est un composant fondamental du Cloud Computing et il fournit de puissantes fonctions aux développeurs Apple et à leurs applications. A la différence du stockage d'informations dans une hiérarchie de fichiers (telle que le Stockage par blocs ou le Stockage de fichiers), un conteneur d'objets se compose uniquement de fichiers et de leurs métadonnées, stockées dans des collections appelées compartiments. Par définition, ces objets sont non modifiables, ce qui les rend parfaits pour les données comme les images, les vidéos et d'autres documents statiques. Pour les données qui changent souvent ou les données relationnelles, vous pouvez utiliser les services de base de données [NoSQL](/docs/swift/data/nosql.html), [Cloudant](/docs/swift/data/cloudant.html) et [SQL](/docs/swift/data/sql.html).

{{site.data.keyword.cos_full_notm}} (COS) est système de stockage qui peut être utilisé pour le stockage de données non structurées à la fois flexibles, abordables et évolutives. Les données sont accessible via des logiciels SDK ou via l'interface utilisateur IBM. Vous pouvez utiliser {{site.data.keyword.cos_full_notm}} pour accéder à vos données non structurées via un portail en libre-service soutenu par des API Restful et des logiciels SDK. 

Selon vos besoins, vous pouvez utiliser {{site.data.keyword.cos_short}} pour les services suivants :

* Référentiel de contenu
* Sauvegarde et restauration
* Archivage de données
* Services de fichier
* Transfert de données

Lorsque vous créez un compartiment, vous devez sélectionner un niveau de résilience (inter-régional ou régional). En fonction de votre sélection, vos données sont dispersés, et stockées dans plusieurs emplacements géographiques.

## API

L'API {{site.data.keyword.cos_full}} est un API REST pour la lecture et l'écriture d'objets. Elle prend en charge un sous-ensemble d'API S3 pour une migration facile des applications vers {{site.data.keyword.cloud_notm}}. Un logiciel SDK S3 peut être utilisé pour optimiser {{site.data.keyword.cos_full}}. Pour plus d'informations, consultez la [Référence d'API {{site.data.keyword.cos_short}}](docs/services/cloud-object-storage/api-reference/about-compatibility-api.html#about-the-ibm-cloud-object-storage-api)

## Sécurité
{: #security}

{{site.data.keyword.cos_full_notm}} est hautement sécurisé. Initialement, seuls les propriétaires du compartiment et de l'objet ont accès au service {{site.data.keyword.cos_full_notm}} qu'ils créent. Il prend également en charge l'authentification d'utilisateur pour l'accès aux données. Utilisez des mécanismes de contrôle d'accès comme mes stratégies de compartiment pour accorder de manière sélective des droits aux utilisateurs et aux applications. Vous pouvez transférer de manière sécurisée le transfert de vos données à {{site.data.keyword.cos_short}} via des noeuds finaux SSL à l'aide du protocole HTTPS. Si vous avez besoin d'une sécurité supplémentaire, vous pouvez utiliser l'option SSE (Server-Side Encryption) ou SSE-C (Server-Side Encryption with Customer-Provided Keys) pour chiffrer les données stockées au repos. {{site.data.keyword.cos_full_notm}} fournit la technologie de chiffrement pour SSE et SSE-C. Vous pouvez aussi utiliser vos propres bibliothèques de chiffrement pour chiffrer les données avant de les stocker dans Cloud Object Storage.

Vous pouvez utiliser les mécanismes de contrôle d'accès suivants pour sécuriser vos données dans {{site.data.keyword.cos_full_notm}}.

**Stratégies IAM (Identity and Access Management)**
IAM permet aux organisations comptant de nombreux employés de créer et de gérer plusieurs utilisateurs sous un seul compte. Grâce aux stratégies IAM, les entreprises peuvent accorder aux utilisateurs IAM le contrôle à leurs compartiments {{site.data.keyword.cos_short}}.

**Listes de contrôle d'accès (ACL)**
Avec les listes de contrôle d'accès, vous pouvez accéder des droits spécifiques (READ, WRITE, par exemple), à des utilisateurs spécifiques pour un compartiment individuel.

Les données au repos sont chiffrées coté fournisseur automatique à l'aide d'un chiffrement AES (Advanced Encryption Standard) 256 bits et d'un hachage SHA-256 (Secure Hash Algorithm). Les données dynamiques sont sécurisées à l'aide d'un chiffrement de qualité opérateur intégré TLS (Transport Layer Security), SSL (Secure Sockets Layer) ou SNMPv3 avec AES.

### Chiffrement

Les données au repos sont chiffrées coté fournisseur automatique à l'aide d'un chiffrement AES (Advanced Encryption Standard) 256 bits et d'un hachage SHA-256 (Secure Hash Algorithm). Les données dynamiques sont sécurisées à l'aide d'un chiffrement de qualité opérateur intégré TLS/ SSL (Transport Layer Security/Secure Sockets Layer) ou SNMPv3 avec AES.

Le chiffrement côté serveur est toujours actif pour les données clients. Par comparaison au hachage qui est requis dans l'authentification, et le codage d'effacement, le chiffrement n'est pas une partie importante du coût de traitement de Cloud Object Storage.

Vous pouvez fournir votre propre clé pour le chiffrement car SSE-C est pris en charge dans {{site.data.keyword.cos_full_notm}}. Toutefois, il vous incombe de gérer et de fournir la clé pour stocker et extraire les données.

## Résilience 

Lorsque vous créez un compartiment, vous devez sélectionner un niveau de résilience (inter-régional ou régional). En fonction de votre sélection, vos données sont dispersés, et stockées dans plusieurs emplacements géographiques.

La résilience régionale est pour la faible latence et vos données sont réparties dans trois centres au sein d'une seule et même région. La résilience est interrégionale pour la disponibilité indispensable à une mission et si vos données sont stockées dans au moins 3 régions différentes. l'interrégionalité offre une résilience géographique et elle est disponible dans plusieurs noeuds finaux. Tenez compte des besoins de votre application pour décider de choisir l'une des deux.

### Géographies

Vous pouvez utiliser {{site.data.keyword.cos_full_notm}} depuis n'importe où dans le monde. Vous pouvez choisir où vous souhaitez stocker vos données au moment de la création du compartiment. Les informations stockées dans IBM COS sont chiffrées et réparties dans plusieurs emplacements géographiques à l'aide des technologies de stockage réparti qui sont fournies par IBM Object Storage Syste. 

Prenez en compte les facteurs suivants pour sélectionner l'emplacement géographique de votre conteneur d'objets, outre le choix entre les options régionales et interrégionales.

**Considérations d'emplacement**:
* Un emplacement qui est distant de vos opérations pour la redondance.
* Un emplacement pour répondre aux exigences juridiques et réglementaires.
* Réduction de la latence d'accès aux données.
* Pris en compte des modèles de tarification.

## Cas d'utilisation et classes de stockage

En fonction de votre cas d'utilisation, vous pouvez réduire les coûts en sélectionnant un plan de service qui répond à vos besoins. Pour les opérations d'archivage qui impliquent un accès minimal au conteneur d'objets, la vitesse et la durabilité d'un objet fréquemment utilisé ne sont pas nécessaires, et cette distinction est reflétée dans la classe de support de stockage et le plan de tarification de vos applications. Les classes de stockage sont définies au niveau du compartiment, de sorte que vous pouvez utiliser une combinaison de plans qui répond à vos besoins. Créez simplement un nouveau compartiment qui est défini pour la classe d'archivage souhaitée.

D'autres informations sur la tarification sont disponibles dans la [documentation relative à la {{site.data.keyword.cos_short}} classe de stockage](/docs/services/cloud-object-storage/help/billing.html#ibm-cos-pricing).

### Exemple de classe de stockage

**Standard**
Ce service est pour les données non structurées qui requièrent un accès fréquent, comme DevOps, des référentiels de contenu d'action et de collaboration.

**Vault**
Ce service est pour les charges de travail avec des données peu fréquemment utilisées, comme les sauvegardes, les archives et les charges de travail de conformité.

**Coffre froid**
Cette option de déploiement est idéale pour les exigences d'accès minimum, la conformité des enregistrements historiques et les sauvegardes à long terme.

**Flex** Déploiement pour les exigences d'accès aux données variables et la protection de votre budget face aux fluctuations de coût imprévues.
Les classes de stockage sont définies au niveau du compartiment. Créez simplement un nouveau compartiment qui est défini pour la classe d'archivage souhaitée.