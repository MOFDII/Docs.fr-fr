---
title: ASP.NET Core sur Nano Server
author: shirhatti
description: "Découvrez comment prendre une application ASP.NET Core existante et la déployer sur une instance de Nano Server exécutant IIS."
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: d9b55fb42088b447451326b7ee573d9bfa5f5941
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="50881-103">ASP.NET Core avec IIS sur Nano Server</span><span class="sxs-lookup"><span data-stu-id="50881-103">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="50881-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="50881-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="50881-105">Dans ce didacticiel, vous allez prendre une application ASP.NET Core existante et la déployer sur une instance de Nano Server exécutant IIS.</span><span class="sxs-lookup"><span data-stu-id="50881-105">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="50881-106">Introduction</span><span class="sxs-lookup"><span data-stu-id="50881-106">Introduction</span></span>

<span data-ttu-id="50881-107">Nano Server est une option d’installation de Windows Server 2016 qui présente un encombrement réduit et offre une meilleure sécurité et une maintenance simplifiée par rapport à Server Core ou à la version Server complète.</span><span class="sxs-lookup"><span data-stu-id="50881-107">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="50881-108">Pour plus d’informations et pour obtenir des liens de téléchargement pour les versions d’évaluation de 180 jours, consultez la [documentation officielle de Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server).</span><span class="sxs-lookup"><span data-stu-id="50881-108">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="50881-109">Il existe trois manières très simples de tester Nano Server.</span><span class="sxs-lookup"><span data-stu-id="50881-109">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="50881-110">Quand vous vous connectez avec votre compte MS :</span><span class="sxs-lookup"><span data-stu-id="50881-110">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="50881-111">Vous pouvez télécharger le fichier ISO de Windows Server 2016 et créer une image Nano Server.</span><span class="sxs-lookup"><span data-stu-id="50881-111">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="50881-112">Téléchargez le disque dur virtuel Nano Server.</span><span class="sxs-lookup"><span data-stu-id="50881-112">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="50881-113">Créez une machine virtuelle dans Azure à l’aide de l’image Nano Server de la galerie Azure.</span><span class="sxs-lookup"><span data-stu-id="50881-113">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="50881-114">Si vous n’avez pas de compte Azure, vous pouvez obtenir une version d’évaluation gratuite de 30 jours.</span><span class="sxs-lookup"><span data-stu-id="50881-114">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="50881-115">Dans ce didacticiel, nous utiliserons la deuxième option, le disque dur virtuel Nano Server prégénéré de Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="50881-115">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="50881-116">Avant de poursuivre ce didacticiel, vous aurez besoin de la [sortie publiée](xref:host-and-deploy/directory-structure) d’une application ASP.NET Core existante.</span><span class="sxs-lookup"><span data-stu-id="50881-116">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="50881-117">Vérifiez que votre application est générée pour s’exécuter dans un processus **64 bits**.</span><span class="sxs-lookup"><span data-stu-id="50881-117">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="50881-118">Configuration de l’instance de Nano Server</span><span class="sxs-lookup"><span data-stu-id="50881-118">Setting up the Nano Server instance</span></span>

<span data-ttu-id="50881-119">[Créez une machine virtuelle à l’aide de Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) sur votre ordinateur de développement à l’aide du disque dur virtuel précédemment téléchargé.</span><span class="sxs-lookup"><span data-stu-id="50881-119">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="50881-120">L’ordinateur vous invitera à définir un mot de passe avant de vous connecter.</span><span class="sxs-lookup"><span data-stu-id="50881-120">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="50881-121">Sur la console de la machine virtuelle, appuyez sur F11 pour définir le mot de passe avant la première connexion.</span><span class="sxs-lookup"><span data-stu-id="50881-121">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="50881-122">Vous devez également vérifier l’adresse IP de votre nouvelle machine virtuelle, soit en vérifiant l’adresse IP fixe de votre serveur DHCP fournie lors de l’approvisionnement de votre machine virtuelle, soit en consultant les paramètres de mise en réseau de la console de récupération Nano Server.</span><span class="sxs-lookup"><span data-stu-id="50881-122">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="50881-123">Supposez que votre nouvelle machine virtuelle s’exécute avec l’adresse IP V4 locale 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="50881-123">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="50881-124">Vous pouvez maintenant la gérer à l’aide de l’accès distant PowerShell, qui est l’unique façon d’administrer intégralement votre Nano Server.</span><span class="sxs-lookup"><span data-stu-id="50881-124">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="50881-125">Connexion à votre instance de Nano Server à l’aide de l’accès distant PowerShell</span><span class="sxs-lookup"><span data-stu-id="50881-125">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="50881-126">Ouvrez une fenêtre PowerShell avec élévation de privilèges pour ajouter votre instance de Nano Server distante à votre liste `TrustedHosts`.</span><span class="sxs-lookup"><span data-stu-id="50881-126">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="50881-127">Remplacez la variable `$nanoServerIpAddress` par l’adresse IP correcte.</span><span class="sxs-lookup"><span data-stu-id="50881-127">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="50881-128">Une fois que vous avez ajouté votre instance de Nano Server à votre `TrustedHosts`, vous pouvez vous y connecter à l’aide de l’accès distant PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50881-128">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="50881-129">Une connexion réussie génère une invite de commandes avec un format ressemblant à ceci : `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="50881-129">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="50881-130">Création d’un partage de fichiers</span><span class="sxs-lookup"><span data-stu-id="50881-130">Creating a file share</span></span>

<span data-ttu-id="50881-131">Créez un partage de fichiers sur le serveur Nano afin que l’application publiée puisse y être copiée.</span><span class="sxs-lookup"><span data-stu-id="50881-131">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="50881-132">Exécutez les commandes suivantes dans la session distante :</span><span class="sxs-lookup"><span data-stu-id="50881-132">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="50881-133">Après avoir exécuté les commandes ci-dessus, vous devez pouvoir accéder à ce partage en visitant `\\192.168.1.10\AspNetCoreSampleForNano` dans l’Explorateur Windows de l’ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="50881-133">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="50881-134">Ouvrir le port dans le pare-feu</span><span class="sxs-lookup"><span data-stu-id="50881-134">Open port in the firewall</span></span>

<span data-ttu-id="50881-135">Exécutez les commandes suivantes dans la session distante pour ouvrir un port dans le pare-feu afin de permettre à IIS d’écouter le trafic TCP sur le port TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="50881-135">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="50881-136">Installation d’IIS</span><span class="sxs-lookup"><span data-stu-id="50881-136">Installing IIS</span></span>

<span data-ttu-id="50881-137">Ajoutez le fournisseur `NanoServerPackage` à partir de PowerShell Gallery.</span><span class="sxs-lookup"><span data-stu-id="50881-137">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="50881-138">Une fois le fournisseur installé et importé, vous pouvez installer les packages Windows.</span><span class="sxs-lookup"><span data-stu-id="50881-138">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="50881-139">Exécutez les commandes suivantes dans la session PowerShell créée précédemment :</span><span class="sxs-lookup"><span data-stu-id="50881-139">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="50881-140">Pour vérifier rapidement si IIS est configuré correctement, accédez à l’URL `http://192.168.1.10/`. Vous devez voir une page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="50881-140">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="50881-141">Quand IIS est installé, un site web nommé `Default Web Site` à l’écoute sur le port 80 est créé par défaut.</span><span class="sxs-lookup"><span data-stu-id="50881-141">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="50881-142">Installation du module ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="50881-142">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="50881-143">Le module ASP.NET Core est un module IIS 7.5+ responsable de la gestion des processus des écouteurs HTTP ASP.NET Core et du transfert des requêtes aux processus qu’il gère.</span><span class="sxs-lookup"><span data-stu-id="50881-143">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="50881-144">À l’heure actuelle, le processus pour installer le module ASP.NET Core pour IIS est manuel.</span><span class="sxs-lookup"><span data-stu-id="50881-144">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="50881-145">Vous devez installer le [bundle d’hébergement .NET Core Windows Server](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) sur un ordinateur standard (autre que Nano).</span><span class="sxs-lookup"><span data-stu-id="50881-145">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="50881-146">Après avoir installé le bundle sur un ordinateur standard, vous devez copier les fichiers suivants sur le partage de fichiers créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="50881-146">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="50881-147">Sur un serveur standard (autre que Nano) avec les services IIS, exécutez les commandes de copie suivantes :</span><span class="sxs-lookup"><span data-stu-id="50881-147">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="50881-148">Remplacez `C:\windows\system32\inetsrv` par `C:\Program Files\IIS Express` sur un ordinateur Windows 10.</span><span class="sxs-lookup"><span data-stu-id="50881-148">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="50881-149">Du côté Nano, vous devez copier les fichiers suivants à partir du partage de fichiers créé précédemment vers les emplacements valides.</span><span class="sxs-lookup"><span data-stu-id="50881-149">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="50881-150">Exécutez les commandes de copie suivantes :</span><span class="sxs-lookup"><span data-stu-id="50881-150">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="50881-151">Exécutez le script suivant dans la session distante :</span><span class="sxs-lookup"><span data-stu-id="50881-151">Run the following script in the remote session:</span></span>

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
> <span data-ttu-id="50881-152">Supprimez les fichiers *aspnetcore.dll* et *aspnetcore_schema.xml* du partage après l’étape ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="50881-152">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="50881-153">Installation de .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="50881-153">Installing .NET Core Framework</span></span>

<span data-ttu-id="50881-154">Si votre application est publiée comme [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core doit être installé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="50881-154">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="50881-155">Utilisez le [script PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) dans une session PowerShell distante pour installer .NET Core sur votre Nano Server.</span><span class="sxs-lookup"><span data-stu-id="50881-155">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="50881-156">Passez la version CLI avec le commutateur `-Version` :</span><span class="sxs-lookup"><span data-stu-id="50881-156">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="50881-157">Publication de l’application</span><span class="sxs-lookup"><span data-stu-id="50881-157">Publishing the application</span></span>

<span data-ttu-id="50881-158">Copiez la sortie publiée de votre application existante à la racine du partage de fichiers.</span><span class="sxs-lookup"><span data-stu-id="50881-158">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="50881-159">Vous devrez peut-être modifier votre fichier *web.config* pour qu’il pointe vers l’endroit où vous avez extrait *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="50881-159">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="50881-160">Vous pouvez également ajouter *dotnet.exe* à votre variable PATH.</span><span class="sxs-lookup"><span data-stu-id="50881-160">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="50881-161">Exemple de fichier *web.config* quand *dotnet.exe* n’est **pas** dans la variable PATH :</span><span class="sxs-lookup"><span data-stu-id="50881-161">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

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

<span data-ttu-id="50881-162">Exécutez les commandes suivantes dans la session distante pour créer un site dans IIS pour l’application publiée sur un port autre que celui du site web par défaut.</span><span class="sxs-lookup"><span data-stu-id="50881-162">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="50881-163">Vous devez également ouvrir ce port pour accéder au web.</span><span class="sxs-lookup"><span data-stu-id="50881-163">You also need to open that port to access the web.</span></span> <span data-ttu-id="50881-164">Ce script utilise le `DefaultAppPool` par souci de simplicité.</span><span class="sxs-lookup"><span data-stu-id="50881-164">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="50881-165">Pour plus d’informations sur l’exécution sous un pool d’applications, consultez [Pools d’applications](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="50881-165">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="50881-166">Problème connu lors de l’exécution de l’interface de ligne de commande .NET Core sur Nano Server et solution de contournement</span><span class="sxs-lookup"><span data-stu-id="50881-166">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="50881-167">Exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="50881-167">Running the application</span></span>

<span data-ttu-id="50881-168">L’application web publiée est accessible dans un navigateur à l’adresse `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="50881-168">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="50881-169">Si vous avez configuré la journalisation comme décrit dans [Création et redirection de journal](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), vous pouvez afficher vos journaux à *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="50881-169">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
