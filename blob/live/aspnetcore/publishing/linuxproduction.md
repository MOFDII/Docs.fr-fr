---
title: "Héberger ASP.NET Core sur Linux avec Nginx"
description: "Décrit comment configurer Nginx comme proxy inverse sur Ubuntu 16.04 pour transférer le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, Proxy inverse
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="60a76-104">Configurer un environnement d’hébergement pour ASP.NET Core sur Linux avec Nginx et déployer dessus</span><span class="sxs-lookup"><span data-stu-id="60a76-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="60a76-105">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="60a76-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="60a76-106">Ce guide explique comment configurer un environnement ASP.NET Core prêt pour la production sur un serveur Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="60a76-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="60a76-107">**Remarque :** Pour Ubuntu 14.04, nous vous recommandons d’utiliser supervisord comme solution pour l’analyse du processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="60a76-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="60a76-108">systemd n’est pas disponible sur Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="60a76-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="60a76-109">Voir la version précédente de ce document</span><span class="sxs-lookup"><span data-stu-id="60a76-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="60a76-110">Ce guide montre comment effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="60a76-110">This guide:</span></span>

* <span data-ttu-id="60a76-111">Placer une application ASP.NET Core existante derrière un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="60a76-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="60a76-112">Configurer le serveur proxy inverse pour transférer les requêtes au serveur web Kestrel</span><span class="sxs-lookup"><span data-stu-id="60a76-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="60a76-113">S’assurer que l’application web s’exécute au démarrage en tant que démon</span><span class="sxs-lookup"><span data-stu-id="60a76-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="60a76-114">Configurer un outil de gestion des processus pour aider à redémarrer l’application web</span><span class="sxs-lookup"><span data-stu-id="60a76-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60a76-115">Prérequis</span><span class="sxs-lookup"><span data-stu-id="60a76-115">Prerequisites</span></span>

1. <span data-ttu-id="60a76-116">Accès à un serveur Ubuntu 16.04 avec un compte d’utilisateur standard disposant de privilèges sudo</span><span class="sxs-lookup"><span data-stu-id="60a76-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="60a76-117">Application ASP.NET Core existante</span><span class="sxs-lookup"><span data-stu-id="60a76-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="60a76-118">Copier votre application</span><span class="sxs-lookup"><span data-stu-id="60a76-118">Copy over your app</span></span>

<span data-ttu-id="60a76-119">Exécutez `dotnet publish` à partir de l’environnement de développement pour empaqueter une application dans un répertoire autonome pouvant s’exécuter sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="60a76-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="60a76-120">Copiez l’application ASP.NET Core sur le serveur à l’aide de n’importe quel outil (SCP, FTP, etc.) qui s’intègre dans votre flux de travail.</span><span class="sxs-lookup"><span data-stu-id="60a76-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="60a76-121">Testez l’application, par exemple :</span><span class="sxs-lookup"><span data-stu-id="60a76-121">Test the app, for example:</span></span>
 - <span data-ttu-id="60a76-122">À partir de la ligne de commande, exécutez `dotnet yourapp.dll`</span><span class="sxs-lookup"><span data-stu-id="60a76-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="60a76-123">Dans un navigateur, accédez à `http://<serveraddress>:<port>` pour vérifier que l’application fonctionne sur Linux.</span><span class="sxs-lookup"><span data-stu-id="60a76-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="60a76-124">Configurer un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="60a76-124">Configure a reverse proxy server</span></span>

<span data-ttu-id="60a76-125">Un proxy inverse est une configuration courante pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="60a76-125">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="60a76-126">Un proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60a76-126">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="60a76-127">Pourquoi utiliser un serveur proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="60a76-127">Why use a reverse proxy server?</span></span>

<span data-ttu-id="60a76-128">Kestrel est une bonne solution pour héberger du contenu dynamique à partir d’ASP.NET ; toutefois, les composants du service web ne sont pas aussi enrichis que ceux offerts par des serveurs comme IIS, Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="60a76-128">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="60a76-129">Un serveur proxy inverse peut décharger du travail comme le traitement du contenu statique, la mise en cache des requêtes, la compression des requêtes et l’arrêt SSL à partir du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="60a76-129">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="60a76-130">Un serveur proxy inverse peut résider sur un ordinateur dédié ou peut être déployé à côté d’un serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="60a76-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="60a76-131">Pour les besoins de ce guide, nous utilisons une seule instance de Nginx.</span><span class="sxs-lookup"><span data-stu-id="60a76-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="60a76-132">Elle s’exécute sur le même serveur, en plus du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="60a76-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="60a76-133">En fonction de vos besoins, vous pouvez choisir une configuration différente.</span><span class="sxs-lookup"><span data-stu-id="60a76-133">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="60a76-134">Les requêtes étant transférées par le proxy inverse, vous devez utiliser l’intergiciel (middleware) `ForwardedHeaders` à partir du package `Microsoft.AspNetCore.HttpOverrides`.</span><span class="sxs-lookup"><span data-stu-id="60a76-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="60a76-135">Cet intergiciel met à jour `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="60a76-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="60a76-136">Quand vous configurez un serveur proxy inverse, l’intergiciel d’authentification a besoin que `UseForwardedHeaders` s’exécute en premier.</span><span class="sxs-lookup"><span data-stu-id="60a76-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="60a76-137">Cet ordre permet d’être sûr que l’intergiciel d’authentification peut consommer les valeurs affectées et générer l’URI de redirection correct.</span><span class="sxs-lookup"><span data-stu-id="60a76-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="60a76-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="60a76-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="60a76-139">Appelez la méthode `UseForwardedHeaders` (dans la méthode `Configure` de *Startup.cs*) avant d’appeler `UseAuthentication` ou un intergiciel de schéma d’authentification similaire :</span><span class="sxs-lookup"><span data-stu-id="60a76-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="60a76-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="60a76-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="60a76-141">Appelez la méthode `UseForwardedHeaders` (dans la méthode `Configure` de *Startup.cs*) avant d’appeler `UseIdentity` et `UseFacebookAuthentication` ou un intergiciel de schéma d’authentification similaire :</span><span class="sxs-lookup"><span data-stu-id="60a76-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

### <a name="install-nginx"></a><span data-ttu-id="60a76-142">Installer Nginx</span><span class="sxs-lookup"><span data-stu-id="60a76-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="60a76-143">Si vous envisagez d’installer des modules Nginx facultatif, vous devrez peut-être générer Nginx à partir de la source.</span><span class="sxs-lookup"><span data-stu-id="60a76-143">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="60a76-144">Utilisez `apt-get` pour installer Nginx.</span><span class="sxs-lookup"><span data-stu-id="60a76-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="60a76-145">Le programme d’installation crée un script d’initialisation System V qui exécute Nginx en tant que démon au démarrage du système.</span><span class="sxs-lookup"><span data-stu-id="60a76-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="60a76-146">Comme il s’agit de l’installation initiale de Nginx, vous devez le démarrer explicitement en exécutant :</span><span class="sxs-lookup"><span data-stu-id="60a76-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="60a76-147">Vérifiez qu’un navigateur affiche la page d’accueil par défaut de Nginx.</span><span class="sxs-lookup"><span data-stu-id="60a76-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="60a76-148">Configurer Nginx</span><span class="sxs-lookup"><span data-stu-id="60a76-148">Configure Nginx</span></span>

<span data-ttu-id="60a76-149">Pour configurer Nginx en tant que proxy inverse pour transférer les requêtes à notre application ASP.NET Core, modifiez le fichier `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="60a76-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="60a76-150">Ouvrez-le dans un éditeur de texte et remplacez le contenu par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="60a76-150">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="60a76-151">Ce fichier de configuration Nginx transfère le trafic public entrant du port `80` au port `5000`.</span><span class="sxs-lookup"><span data-stu-id="60a76-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="60a76-152">Une fois que vous avez fini de modifier votre configuration Nginx, vous pouvez exécuter `sudo nginx -t` pour vérifier la syntaxe de vos fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="60a76-152">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="60a76-153">Si le test de fichier de configuration réussit, vous pouvez demander à Nginx d’appliquer les modifications en exécutant `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="60a76-153">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="60a76-154">Surveillance de notre application</span><span class="sxs-lookup"><span data-stu-id="60a76-154">Monitoring our application</span></span>

<span data-ttu-id="60a76-155">Nginx est désormais configuré pour transférer les requêtes faites à `http://yourhost:80` à l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="60a76-155">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="60a76-156">Toutefois, Nginx n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="60a76-156">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="60a76-157">Vous pouvez utiliser *systemd* et créer un fichier de service pour démarrer et surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="60a76-157">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="60a76-158">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="60a76-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="60a76-159">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="60a76-159">Create the service file</span></span>

<span data-ttu-id="60a76-160">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="60a76-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="60a76-161">Voici un exemple de fichier de service pour notre application :</span><span class="sxs-lookup"><span data-stu-id="60a76-161">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="60a76-162">**Remarque :** Si l’utilisateur *www-data* n’est pas utilisé par votre configuration, vous devez d’abord créer l’utilisateur défini ici et l’affecter en tant que propriétaire des fichiers.</span><span class="sxs-lookup"><span data-stu-id="60a76-162">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="60a76-163">Enregistrez le fichier et activez le service.</span><span class="sxs-lookup"><span data-stu-id="60a76-163">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="60a76-164">Démarrez le service et vérifiez qu’il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="60a76-164">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="60a76-165">Le proxy inverse étant configuré et Kestrel étant géré par le biais de systemd, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur l’ordinateur local à l’adresse `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="60a76-165">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="60a76-166">Elle est également accessible à partir d’un ordinateur distant, sauf en cas de blocage par un pare-feu.</span><span class="sxs-lookup"><span data-stu-id="60a76-166">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="60a76-167">Si l’on inspecte les en-têtes de réponse, on constate que l’en-tête `Server` indique que l’application ASP.NET Core est traitée par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="60a76-167">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="60a76-168">Affichage des journaux</span><span class="sxs-lookup"><span data-stu-id="60a76-168">Viewing logs</span></span>

<span data-ttu-id="60a76-169">Puisque l’application web utilisant Kestrel est gérée à l’aide de `systemd`, tous les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="60a76-169">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="60a76-170">Toutefois, ce journal inclut toutes les entrées pour tous les services et les processus gérés par `systemd`.</span><span class="sxs-lookup"><span data-stu-id="60a76-170">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="60a76-171">Pour afficher les éléments propres à `kestrel-hellomvc.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="60a76-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="60a76-172">Si vous souhaitez appliquer un filtrage supplémentaire, des options temporelles telles que `--since today`, `--until 1 hour ago` ou une combinaison de ces options peut réduire la quantité d’entrées retournées.</span><span class="sxs-lookup"><span data-stu-id="60a76-172">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="60a76-173">Sécurisation de votre application</span><span class="sxs-lookup"><span data-stu-id="60a76-173">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="60a76-174">Activer AppArmor</span><span class="sxs-lookup"><span data-stu-id="60a76-174">Enable AppArmor</span></span>

<span data-ttu-id="60a76-175">Linux Security Modules (LSM) est un framework qui fait partie du noyau Linux depuis Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="60a76-175">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="60a76-176">LSM prend en charge différentes implémentations de modules de sécurité.</span><span class="sxs-lookup"><span data-stu-id="60a76-176">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="60a76-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) est un LSM qui implémente un système de contrôle d’accès obligatoire permettant de confiner le programme à un ensemble limité de ressources.</span><span class="sxs-lookup"><span data-stu-id="60a76-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="60a76-178">Vérifiez qu’AppArmor est activé et configuré correctement.</span><span class="sxs-lookup"><span data-stu-id="60a76-178">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="60a76-179">Configuration de notre pare-feu</span><span class="sxs-lookup"><span data-stu-id="60a76-179">Configuring our firewall</span></span>

<span data-ttu-id="60a76-180">Fermez tous les ports externes qui ne sont pas en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="60a76-180">Close off all external ports that are not in use.</span></span> <span data-ttu-id="60a76-181">Uncomplicated firewall (ufw) fournit une interface de ligne de commande pour `iptables` afin de configurer le pare-feu.</span><span class="sxs-lookup"><span data-stu-id="60a76-181">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="60a76-182">Vérifiez que `ufw` est configuré pour autoriser le trafic sur les ports dont vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="60a76-182">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="60a76-183">Sécurisation de Nginx</span><span class="sxs-lookup"><span data-stu-id="60a76-183">Securing Nginx</span></span>

<span data-ttu-id="60a76-184">La distribution par défaut de Nginx n’active pas le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="60a76-184">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="60a76-185">Pour activer des fonctionnalités de sécurité supplémentaires, générez à partir de la source.</span><span class="sxs-lookup"><span data-stu-id="60a76-185">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="60a76-186">Télécharger la source et installer les dépendances de build</span><span class="sxs-lookup"><span data-stu-id="60a76-186">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="60a76-187">Changer le nom de la réponse Nginx</span><span class="sxs-lookup"><span data-stu-id="60a76-187">Change the Nginx response name</span></span>

<span data-ttu-id="60a76-188">Modifiez *src/http/ngx_http_header_filter_module.c* :</span><span class="sxs-lookup"><span data-stu-id="60a76-188">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="60a76-189">Configurer les options et générer</span><span class="sxs-lookup"><span data-stu-id="60a76-189">Configure the options and build</span></span>

<span data-ttu-id="60a76-190">La bibliothèque PCRE est obligatoire pour les expressions régulières.</span><span class="sxs-lookup"><span data-stu-id="60a76-190">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="60a76-191">Les expressions régulières sont utilisées dans la directive d’emplacement pour ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="60a76-191">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="60a76-192">http_ssl_module ajoute la prise en charge du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="60a76-192">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="60a76-193">Pour renforcer votre application, vous pouvez utiliser un pare-feu d’application web tel que *ModSecurity*.</span><span class="sxs-lookup"><span data-stu-id="60a76-193">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="60a76-194">Configurer le protocole SSL</span><span class="sxs-lookup"><span data-stu-id="60a76-194">Configure SSL</span></span>

* <span data-ttu-id="60a76-195">Configurez votre serveur pour qu’il écoute le trafic HTTPS sur le port `443` en spécifiant un certificat valide émis par une autorité de certificat approuvée.</span><span class="sxs-lookup"><span data-stu-id="60a76-195">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="60a76-196">Renforcez la sécurité en appliquant certaines des pratiques mentionnées dans le fichier */etc/nginx/nginx.conf* suivant.</span><span class="sxs-lookup"><span data-stu-id="60a76-196">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="60a76-197">Vous pouvez par exemple choisir un chiffrement plus fort et rediriger tout le trafic sur HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="60a76-197">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="60a76-198">L’ajout d’un en-tête `HTTP Strict-Transport-Security` (HSTS) garantit que toutes les requêtes ultérieures du client s’effectuent uniquement par le biais du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="60a76-198">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="60a76-199">N’ajoutez pas l’en-tête Strict-Transport-Security ou choisissez une valeur `max-age` appropriée si vous prévoyez de désactiver le protocole SSL ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="60a76-199">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="60a76-200">Ajoutez le fichier de configuration */etc/nginx/proxy.conf* :</span><span class="sxs-lookup"><span data-stu-id="60a76-200">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="60a76-201">Modifiez le fichier de configuration */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="60a76-201">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="60a76-202">Dans l’exemple, les sections `http` et `server` figurent dans un même fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="60a76-202">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="60a76-203">Sécuriser Nginx contre le détournement de clic</span><span class="sxs-lookup"><span data-stu-id="60a76-203">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="60a76-204">Le détournement de clic, ou clickjacking, est une technique malveillante visant à recueillir les clics d’un utilisateur infecté.</span><span class="sxs-lookup"><span data-stu-id="60a76-204">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="60a76-205">Elle pousse la victime (le visiteur) à cliquer sur un site infecté.</span><span class="sxs-lookup"><span data-stu-id="60a76-205">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="60a76-206">Utilisez X-FRAME-OPTIONS pour sécuriser votre site.</span><span class="sxs-lookup"><span data-stu-id="60a76-206">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="60a76-207">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="60a76-207">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="60a76-208">Ajoutez la ligne `add_header X-Frame-Options "SAMEORIGIN";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="60a76-208">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="60a76-209">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="60a76-209">MIME-type sniffing</span></span>

<span data-ttu-id="60a76-210">Cet en-tête empêche la plupart des navigateurs de détourner le type MIME d’une réponse et de remplacer le type de contenu déclaré, car l’en-tête indique au navigateur qu’il ne doit pas substituer le type de contenu de la réponse.</span><span class="sxs-lookup"><span data-stu-id="60a76-210">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="60a76-211">Avec l’option `nosniff`, si le serveur indique que le contenu est « text/html », le navigateur le restitue en tant que « text/html ».</span><span class="sxs-lookup"><span data-stu-id="60a76-211">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="60a76-212">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="60a76-212">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="60a76-213">Ajoutez la ligne `add_header X-Content-Type-Options "nosniff";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="60a76-213">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
