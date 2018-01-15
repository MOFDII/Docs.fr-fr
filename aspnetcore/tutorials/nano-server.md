---
title: ASP.NET Core sur Nano Server
author: shirhatti
description: "Découvrez comment prendre une application ASP.NET Core existante et la déployer sur une instance de Nano Server exécutant IIS."
keywords: ASP.NET Core, nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: f30e911703d5c36d076872f91d4b2fafeefb91f5
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="f7da6-104">ASP.NET Core avec IIS sur Nano Server</span><span class="sxs-lookup"><span data-stu-id="f7da6-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="f7da6-105">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="f7da6-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="f7da6-106">Dans ce didacticiel, vous allez prendre une application ASP.NET Core existante et la déployer sur une instance de Nano Server exécutant IIS.</span><span class="sxs-lookup"><span data-stu-id="f7da6-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="f7da6-107">Introduction</span><span class="sxs-lookup"><span data-stu-id="f7da6-107">Introduction</span></span>

<span data-ttu-id="f7da6-108">Nano Server est une option d’installation de Windows Server 2016 qui présente un encombrement réduit et offre une meilleure sécurité et une maintenance simplifiée par rapport à Server Core ou à la version Server complète.</span><span class="sxs-lookup"><span data-stu-id="f7da6-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="f7da6-109">Pour plus d’informations et pour obtenir des liens de téléchargement pour les versions d’évaluation de 180 jours, consultez la [documentation officielle de Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server).</span><span class="sxs-lookup"><span data-stu-id="f7da6-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="f7da6-110">Il existe trois manières très simples de tester Nano Server.</span><span class="sxs-lookup"><span data-stu-id="f7da6-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="f7da6-111">Quand vous vous connectez avec votre compte MS :</span><span class="sxs-lookup"><span data-stu-id="f7da6-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="f7da6-112">Vous pouvez télécharger le fichier ISO de Windows Server 2016 et créer une image Nano Server.</span><span class="sxs-lookup"><span data-stu-id="f7da6-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="f7da6-113">Téléchargez le disque dur virtuel Nano Server.</span><span class="sxs-lookup"><span data-stu-id="f7da6-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="f7da6-114">Créez une machine virtuelle dans Azure à l’aide de l’image Nano Server de la galerie Azure.</span><span class="sxs-lookup"><span data-stu-id="f7da6-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="f7da6-115">Si vous n’avez pas de compte Azure, vous pouvez obtenir une version d’évaluation gratuite de 30 jours.</span><span class="sxs-lookup"><span data-stu-id="f7da6-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="f7da6-116">Dans ce didacticiel, nous utiliserons la deuxième option, le disque dur virtuel Nano Server prégénéré de Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="f7da6-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="f7da6-117">Avant de poursuivre ce didacticiel, vous aurez besoin de la [sortie publiée](xref:host-and-deploy/directory-structure) d’une application ASP.NET Core existante.</span><span class="sxs-lookup"><span data-stu-id="f7da6-117">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="f7da6-118">Vérifiez que votre application est générée pour s’exécuter dans un processus **64 bits**.</span><span class="sxs-lookup"><span data-stu-id="f7da6-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="f7da6-119">Configuration de l’instance de Nano Server</span><span class="sxs-lookup"><span data-stu-id="f7da6-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="f7da6-120">[Créez une machine virtuelle à l’aide de Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) sur votre ordinateur de développement à l’aide du disque dur virtuel précédemment téléchargé.</span><span class="sxs-lookup"><span data-stu-id="f7da6-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="f7da6-121">L’ordinateur vous invitera à définir un mot de passe avant de vous connecter.</span><span class="sxs-lookup"><span data-stu-id="f7da6-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="f7da6-122">Sur la console de la machine virtuelle, appuyez sur F11 pour définir le mot de passe avant la première connexion.</span><span class="sxs-lookup"><span data-stu-id="f7da6-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="f7da6-123">Vous devez également vérifier l’adresse IP de votre nouvelle machine virtuelle, soit en vérifiant l’adresse IP fixe de votre serveur DHCP fournie lors de l’approvisionnement de votre machine virtuelle, soit en consultant les paramètres de mise en réseau de la console de récupération Nano Server.</span><span class="sxs-lookup"><span data-stu-id="f7da6-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="f7da6-124">Supposez que votre nouvelle machine virtuelle s’exécute avec l’adresse IP V4 locale 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="f7da6-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="f7da6-125">Vous pouvez maintenant la gérer à l’aide de l’accès distant PowerShell, qui est l’unique façon d’administrer intégralement votre Nano Server.</span><span class="sxs-lookup"><span data-stu-id="f7da6-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="f7da6-126">Connexion à votre instance de Nano Server à l’aide de l’accès distant PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7da6-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="f7da6-127">Ouvrez une fenêtre PowerShell avec élévation de privilèges pour ajouter votre instance de Nano Server distante à votre liste `TrustedHosts`.</span><span class="sxs-lookup"><span data-stu-id="f7da6-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="f7da6-128">Remplacez la variable `$nanoServerIpAddress` par l’adresse IP correcte.</span><span class="sxs-lookup"><span data-stu-id="f7da6-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="f7da6-129">Une fois que vous avez ajouté votre instance de Nano Server à votre `TrustedHosts`, vous pouvez vous y connecter à l’aide de l’accès distant PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7da6-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="f7da6-130">Une connexion réussie génère une invite de commandes avec un format ressemblant à ceci : `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="f7da6-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="f7da6-131">Création d’un partage de fichiers</span><span class="sxs-lookup"><span data-stu-id="f7da6-131">Creating a file share</span></span>

<span data-ttu-id="f7da6-132">Créez un partage de fichiers sur le serveur Nano afin que l’application publiée puisse y être copiée.</span><span class="sxs-lookup"><span data-stu-id="f7da6-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="f7da6-133">Exécutez les commandes suivantes dans la session distante :</span><span class="sxs-lookup"><span data-stu-id="f7da6-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="f7da6-134">Après avoir exécuté les commandes ci-dessus, vous devez pouvoir accéder à ce partage en visitant `\\192.168.1.10\AspNetCoreSampleForNano` dans l’Explorateur Windows de l’ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="f7da6-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="f7da6-135">Ouvrir le port dans le pare-feu</span><span class="sxs-lookup"><span data-stu-id="f7da6-135">Open port in the firewall</span></span>

<span data-ttu-id="f7da6-136">Exécutez les commandes suivantes dans la session distante pour ouvrir un port dans le pare-feu afin de permettre à IIS d’écouter le trafic TCP sur le port TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="f7da6-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="f7da6-137">Installation d’IIS</span><span class="sxs-lookup"><span data-stu-id="f7da6-137">Installing IIS</span></span>

<span data-ttu-id="f7da6-138">Ajoutez le fournisseur `NanoServerPackage` à partir de PowerShell Gallery.</span><span class="sxs-lookup"><span data-stu-id="f7da6-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="f7da6-139">Une fois le fournisseur installé et importé, vous pouvez installer les packages Windows.</span><span class="sxs-lookup"><span data-stu-id="f7da6-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="f7da6-140">Exécutez les commandes suivantes dans la session PowerShell créée précédemment :</span><span class="sxs-lookup"><span data-stu-id="f7da6-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="f7da6-141">Pour vérifier rapidement si IIS est configuré correctement, accédez à l’URL `http://192.168.1.10/`. Vous devez voir une page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="f7da6-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="f7da6-142">Quand IIS est installé, un site web nommé `Default Web Site` à l’écoute sur le port 80 est créé par défaut.</span><span class="sxs-lookup"><span data-stu-id="f7da6-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="f7da6-143">Installation du module ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="f7da6-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="f7da6-144">Le module ASP.NET Core est un module IIS 7.5+ responsable de la gestion des processus des écouteurs HTTP ASP.NET Core et du transfert des requêtes aux processus qu’il gère.</span><span class="sxs-lookup"><span data-stu-id="f7da6-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="f7da6-145">À l’heure actuelle, le processus pour installer le module ASP.NET Core pour IIS est manuel.</span><span class="sxs-lookup"><span data-stu-id="f7da6-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="f7da6-146">Vous devez installer le [bundle d’hébergement .NET Core Windows Server](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) sur un ordinateur standard (autre que Nano).</span><span class="sxs-lookup"><span data-stu-id="f7da6-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="f7da6-147">Après avoir installé le bundle sur un ordinateur standard, vous devez copier les fichiers suivants sur le partage de fichiers créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="f7da6-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="f7da6-148">Sur un serveur standard (autre que Nano) avec les services IIS, exécutez les commandes de copie suivantes :</span><span class="sxs-lookup"><span data-stu-id="f7da6-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="f7da6-149">Remplacez `C:\windows\system32\inetsrv` par `C:\Program Files\IIS Express` sur un ordinateur Windows 10.</span><span class="sxs-lookup"><span data-stu-id="f7da6-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="f7da6-150">Du côté Nano, vous devez copier les fichiers suivants à partir du partage de fichiers créé précédemment vers les emplacements valides.</span><span class="sxs-lookup"><span data-stu-id="f7da6-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="f7da6-151">Exécutez les commandes de copie suivantes :</span><span class="sxs-lookup"><span data-stu-id="f7da6-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="f7da6-152">Exécutez le script suivant dans la session distante :</span><span class="sxs-lookup"><span data-stu-id="f7da6-152">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="f7da6-153">Supprimez les fichiers *aspnetcore.dll* et *aspnetcore_schema.xml* du partage après l’étape ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="f7da6-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="f7da6-154">Installation de .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="f7da6-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="f7da6-155">Si votre application est publiée comme [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core doit être installé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f7da6-155">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="f7da6-156">Utilisez le [script PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) dans une session PowerShell distante pour installer .NET Core sur votre Nano Server.</span><span class="sxs-lookup"><span data-stu-id="f7da6-156">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="f7da6-157">Passez la version CLI avec le commutateur `-Version` :</span><span class="sxs-lookup"><span data-stu-id="f7da6-157">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="f7da6-158">Publication de l’application</span><span class="sxs-lookup"><span data-stu-id="f7da6-158">Publishing the application</span></span>

<span data-ttu-id="f7da6-159">Copiez la sortie publiée de votre application existante à la racine du partage de fichiers.</span><span class="sxs-lookup"><span data-stu-id="f7da6-159">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="f7da6-160">Vous devrez peut-être modifier votre fichier *web.config* pour qu’il pointe vers l’endroit où vous avez extrait *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="f7da6-160">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="f7da6-161">Vous pouvez également ajouter *dotnet.exe* à votre variable PATH.</span><span class="sxs-lookup"><span data-stu-id="f7da6-161">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="f7da6-162">Exemple de fichier *web.config* quand *dotnet.exe* n’est **pas** dans la variable PATH :</span><span class="sxs-lookup"><span data-stu-id="f7da6-162">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="f7da6-163">Exécutez les commandes suivantes dans la session distante pour créer un site dans IIS pour l’application publiée sur un port autre que celui du site web par défaut.</span><span class="sxs-lookup"><span data-stu-id="f7da6-163">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="f7da6-164">Vous devez également ouvrir ce port pour accéder au web.</span><span class="sxs-lookup"><span data-stu-id="f7da6-164">You also need to open that port to access the web.</span></span> <span data-ttu-id="f7da6-165">Ce script utilise le `DefaultAppPool` par souci de simplicité.</span><span class="sxs-lookup"><span data-stu-id="f7da6-165">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="f7da6-166">Pour plus d’informations sur l’exécution sous un pool d’applications, consultez [Pools d’applications](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="f7da6-166">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="f7da6-167">Problème connu lors de l’exécution de l’interface de ligne de commande .NET Core sur Nano Server et solution de contournement</span><span class="sxs-lookup"><span data-stu-id="f7da6-167">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="f7da6-168">Exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="f7da6-168">Running the application</span></span>

<span data-ttu-id="f7da6-169">L’application web publiée est accessible dans un navigateur à l’adresse `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="f7da6-169">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="f7da6-170">Si vous avez configuré la journalisation comme décrit dans [Création et redirection de journal](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), vous pouvez afficher vos journaux à *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="f7da6-170">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
