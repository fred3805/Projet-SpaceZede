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

```

**5 - Création des GPO :**


**6 - Création des scripts :**


<a href="README.md">(Retourner au sommaire)</a>
