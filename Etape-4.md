## Configuration du domaine
<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  

**1 - Création des OUs :**

```
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

```

**4 - Création des dossiers partagés :**
```
<#
Description :
	Script "AddSharedFolders" permettant de créer les dossiers partagés avec les bons droits.
Auteur :
	github.com/rikiya-gabimaru
Version :
	0.1
Révision :
	- 0.1 (20/01/2025) : Version Initiale
#>

# Définir le chemin de base où les dossiers partagés seront créés.
$BasePath = "C:\Shares"

# Connexion à l'Active Directory.
Import-Module ActiveDirectory

# Définir l'OU racine où seront crées les dossiers partagés.
$RootOu = "OU=SpaceZede,DC=SpaceZede,DC=local"

# Récupérer toutes les OUs correspondant aux noms de services sous l'OU racine.
$ServiceOUs = Get-ADOrganizationalUnit -Filter * -SearchBase $RootOu -SearchScope OneLevel

foreach ($ServiceOu in $ServiceOus) {
	$ServiceName = $ServiceOU.Name
	# Créer un dossier pour le service si il n'existe pas déjà.
	$ServiceFolderPath = Join-Path -Path $BasePath -ChildPath $ServiceName
	if (-not (Test-Path $ServiceFolderPath)) {
        New-Item -Path $ServiceFolderPath -ItemType Directory
	}
	# Récupérer le groupe dans l'OU "Groupes" de l'OU de service dans lequel il se trouve.
	$GroupOU = "OU=Groupes," + $ServiceOu.DistinguishedName
	$Groups = Get-ADGroup -Filter * -SearchBase $GroupOu
	# Accorder des permissions aux groupes sur le dossier du service.
	foreach ($Group in $Groups) {
		$GroupName = $Group.Name
		$Acl = Get-Acl -Path $ServiceFolderPath
		$AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule(
			"$GroupName", "Modify", "ContainerInherit,ObjectInherit", "None", "Allow"
		)
		$acl.AddAccessRule($accessRule)
		Set-Acl -Path $ServiceFolderPath -AclObject $acl
	}

	# Récupérer les utilisateurs dans l'OU "Utilisateurs" de l'OU de service dans lequel ils se trouvent.
	$UserOu = "OU=Utilisateurs," + $ServiceOu.DistinguishedName
	$Users = Get-ADUser -Filter * -SearchBase $UserOu

	# Créer un dossier partagé pour chaque utilisateur
	foreach ($User in $Users) {
		$UserName = $User.SamAccountName
		$UserFolderPath = Join-Path -Path $BasePath -ChildPath $UserName

		# Créer un dossier pour l'utilisateur si il n'existe pas déjà.
		if (-not (Test-Path $userFolderPath)) {
			New-Item -Path $UserFolderPath -ItemType Directory
		}

		# Accorder les permissions à l'utilisateur sur son propre dossier.
		$Acl = Get-Acl -Path $UserFolderPath
		$AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule(
			"$UserName", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow"
		)
		$Acl.AddAccessRule($accessRule)
		Set-Acl -Path $UserFolderPath -AclObject $Acl
	}
}
Write-Host "Dossiers partagés créés et permissions attribuées."
```

**5 - Création des GPO :**


**6 - Création des scripts :**


<a href="README.md">(Retourner au sommaire)</a>
