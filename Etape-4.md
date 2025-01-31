## Configuration du domaine
<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  

**1 - Création des OUs :**

```
<#
Description :
	Script "AddOu" permettant d'ajouter des OUs de services avec leurs sous-OUs.
Auteur :
	github.com/rikiya-gabimaru
Version :
	0.1
Révision :
	- 0.1 (20/01/2025) : Version Initiale
#>

# Définir le chemin de l'OU parent où seront ajouter les OUs de service.
$OuParent = "OU=SpaceZede,DC=SpaceZede,DC=local"

# Liste des services à créer.
$Services = @("Direction", "Finance", "Marketing", "Production")

# Création des OUs pour chaque service.
foreach ($Service in $Services) {
	# Définir le chemin complet pour l'OU du service.
	$OuServiceDN = "OU=$Service,$OuParent"
    
	# Créer l'OU pour le service.
	try {
		New-ADOrganizationalUnit -Name $Service -Path $OuParent -ErrorAction Stop
		Write-Host "Création de l'OU $Service réussie."
	}
	catch {
		Write-Host "Erreur lors de la création de l'OU $service."
	}

	# Créer les sous-OUs Ordinateurs, Utilisateurs, Groupes dans les OUs service.
	foreach ($SubOU in @("Ordinateurs", "Utilisateurs", "Groupes")) {
		$SubOuDN = "OU=$SubOu,$OuServiceDN"
        
		try {
			New-ADOrganizationalUnit -Name $SubOu -Path $OuServiceDN -ErrorAction Stop
			Write-Host "Création de l'OU $SubOu dans $Service réussie."
		}
		catch {
			Write-Host "Erreur lors de la création de l'OU $SubOu dans $Service."
		}
	}
}

Write-Host "Les OUs ont été traitées."
```

**2 - Création des utilisateurs :**
```
<#
Description :
	Script "AddUsers" permettant de créer les utilisateurs dans les OUs "Utilisateurs" se trouvant dans leur OU de service.
Auteur :
	github.com/rikiya-gabimaru
Version :
	0.1
Révision :
	- 0.1 (20/01/2025) : Version Initiale
#>

# Chemin du fichier CSV listant les utilisateurs.
$CsvPath = "C:\Script\AddUsers.csv"

# Importation du fichier CSV.
$Users = Import-Csv -Path $CsvPath

# Création des utilisateurs se trouvant dans le fichier du CSV.
foreach ($User in $Users) {
	# Extraction des informations du fichier CSV.
	$Prenom = $User.Prenom
	$Nom = $User.Nom
	$Service = $User.Service
	$Email = $User.Mail # Utiliser directement l'email du fichier CSV

	# Définir le nom complet et le nom d'utilisateur (UPN).
	$NomComplet = "$Prenom $Nom"
	$UserPrincipalName = $Email  # Utiliser directement l'email du fichier CSV
	$Login = "$Prenom.$Nom"

	# Définir l'OU où l'utilisateur sera créé.
	$OuPath = "OU=Utilisateurs,OU=$service,OU=SpaceZede,DC=SpaceZede,DC=local"

	# Créer l'utilisateur dans l'AD.
	New-ADUser -SamAccountName $Login `
		-UserPrincipalName $UserPrincipalName `
		-Name $NomComplet `
		-GivenName $Prenom `
		-Surname $Nom `
		-DisplayName $NomComplet `
		-EmailAddress $Email `
		-Department $Service `
		-AccountPassword (ConvertTo-SecureString "Azerty1*" -AsPlainText -Force) `
		-Enabled $true `
		-Path $OuPath `
		-PassThru

	Write-Host "Utilisateur $nomComplet créé avec succès dans l'OU $OuPath"
}
```

**3 - Création des groupes :**
```
<#
Description :
	Script "AddGroups" permettant de créer les groupes globaux de sécurité.
Auteur :
	github.com/rikiya-gabimaru
Version :
	0.1
Révision :
	- 0.1 (20/01/2025) : Version Initiale
#>

# Définir l'OU racine où se fera les checks et les ajouts.
$RootOu = "OU=SpaceZede,DC=SpaceZede,DC=local"

# Récupérer les noms des OU services sous l'OU racine.
$Services = Get-ADOrganizationalUnit -Filter * -SearchBase "LDAP://$RootOu"

foreach ($Service in $Services) {
	# Récupérer les OUs "Groupes" et "Utilisateurs" sous chaque service
	$GroupOu = Get-ADOrganizationalUnit -Filter {Name -eq "Groupes"} -SearchBase $Service.DistinguishedName
	$UserOu = Get-ADOrganizationalUnit -Filter {Name -eq "Utilisateurs"} -SearchBase $Service.DistinguishedName

	# Vérifier si les OUs "Groupes" et "Utilisateurs" existent.
	if ($GroupOu -and $UserOu) {
		# Nom du groupe à créer sous la forme "GG_<Nom du service>"
		$GroupName = "GG_" + $Service.Name

		# Vérifier si le groupe existe déjà, sinon le créer.
		$Group = Get-ADGroup -Filter {Name -eq $GroupName} -SearchBase $GroupOu.DistinguishedName
		if (-not $Group) {
			Write-Host "Création du groupe : $GroupName"
			New-ADGroup -Name $GroupName -GroupScope Global -GroupCategory Security -Path $GroupOu.DistinguishedName
		}

		# Récupérer tous les utilisateurs dans l'OU "Utilisateurs" du même service.
		$Users = Get-ADUser -Filter * -SearchBase $UserOu.DistinguishedName

		# Ajouter les utilisateurs comme membres du groupe.
		foreach ($User in $Users) {
			Write-Host "Ajout de l'utilisateur $($User.SamAccountName) au groupe $GroupName"
			Add-ADGroupMember -Identity $GroupName -Members $User
		}
	} else {
		Write-Host "OU 'Groupes' ou 'Utilisateurs' manquante dans le service : $($Service.Name)."
	}
}

```

**4 - Création des dossiers partagés :**
```

```

**5 - Création des GPO :**


**6 - Création des scripts :**


<a href="README.md">(Retourner au sommaire)</a>
