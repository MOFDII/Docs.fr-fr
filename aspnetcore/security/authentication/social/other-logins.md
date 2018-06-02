---
title: Brève enquête d’autres fournisseurs d’authentification
author: rick-anderson
manager: wpickett
ms.author: riande
ms.date: 11/03/2016
ms.prod: asp.net-core
ms.topic: article
uid: security/authentication/otherlogins
ms.openlocfilehash: 25c88ae2fdd210c827f3f71d90b1bcca164d86af
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729517"
---
# <a name="short-survey-of-other-authentication-providers"></a>Brève enquête d’autres fournisseurs d’authentification

<a name="security-authentication-other-logins"></a>

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), et [Valeriy Novytskyy](https://github.com/01binary)

Sont définies ici les instructions pour d’autres fournisseurs OAuth courants. Les packages NuGet tiers tels que ceux gérés par [aspnet-cotisation](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth)peuvent être utilisés pour compléter les fournisseurs d’authentification implémentés par l’équipe ASP.NET Core.

* Configurer **LinkedIn** connectez-vous : [ https://www.linkedin.com/developer/apps ](https://www.linkedin.com/developer/apps). Consultez [étapes officiels](https://developer.linkedin.com/docs/oauth2).

* Configurer **Instagram** connectez-vous : [ https://www.instagram.com/developer/register/ ](https://www.instagram.com/developer/register/). Consultez [étapes officiels](https://www.instagram.com/developer/authentication/).

* Configurer **Reddit** connectez-vous : [ https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps ](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps). Consultez [étapes officiels](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).

* Configurer **Github** connectez-vous : [ https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew ](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew). Consultez [étapes officiels](https://developer.github.com/v3/oauth/).

* Configurer **Yahoo** connectez-vous : [ https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F ](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F). Consultez [étapes officiels](https://developer.yahoo.com/bbauth/user.html).

* Configurer **Tumblr** connectez-vous : [ https://www.tumblr.com/oauth/apps ](https://www.tumblr.com/oauth/apps). Consultez [étapes officiels](https://www.tumblr.com/docs/api/v2#auth).

* Configurer **Pinterest** connectez-vous : [ https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F ](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F). Consultez [étapes officiels](https://developers.pinterest.com/docs/api/overview/?).

* Configurer **Pocket** connectez-vous : [ https://getpocket.com/developer/apps/new ](https://getpocket.com/developer/apps/new). Consultez [étapes officiels](https://getpocket.com/developer/docs/authentication).

* Configurer **Flickr** connectez-vous : [ https://www.flickr.com/services/apps/create ](https://www.flickr.com/services/apps/create). Consultez [étapes officiels](https://www.flickr.com/services/api/auth.oauth.html).

* Configurer **Dribble** connectez-vous : [ https://dribbble.com/signup ](https://dribbble.com/signup). Consultez [étapes officiels](http://developer.dribbble.com/v1/oauth/).

* Configurer **Vimeo** connectez-vous : [ https://vimeo.com/join ](https://vimeo.com/join). Consultez [étapes officiels](https://developer.vimeo.com/api/authentication).

* Configurer **SoundCloud** connectez-vous : [ https://soundcloud.com/you/apps/new ](https://soundcloud.com/you/apps/new). Consultez [étapes officiels](https://developers.soundcloud.com/blog/we-love-oauth-2).

* Configurer **VK** connectez-vous : [ https://vk.com/apps?act=manage ](https://vk.com/apps?act=manage). Consultez [étapes officiels](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).

## <a name="multiple-authentication-providers"></a>Plusieurs fournisseurs d’authentification

[!INCLUDE[](~/includes/chain-auth-providers.md)]
