# test


# Importer le module Active Directory
Import-Module ActiveDirectory

# Définir le domaine cible et le nombre de jours avant expiration
$Domaine = "tssr.info"
$Jours = 30

# Calculer la date limite (aujourd'hui + $Jours)
$DateLimite = (Get-Date).AddDays($Jours)

Write-Host "Recherche des utilisateurs dans le domaine $Domaine avec une expiration dans moins de $Jours jours..."

try {
    # Récupérer tous les utilisateurs du domaine
    $Utilisateurs = Get-ADUser -Filter {AccountExpirationDate -le $DateLimite -and AccountExpirationDate -ne $null} -Server $Domaine -Properties AccountExpirationDate, DisplayName, EmailAddress
    
    if ($Utilisateurs) {
        Write-Host "Utilisateurs trouvés avec des comptes expirant dans moins de $Jours jours :"
        foreach ($Utilisateur in $Utilisateurs) {
            Write-Host "Nom complet : $($Utilisateur.DisplayName)"
            Write-Host "Email       : $($Utilisateur.EmailAddress)"
            Write-Host "Expiration  : $($Utilisateur.AccountExpirationDate)"
            Write-Host "-------------------------------------"
        }
    } else {
        Write-Host "Aucun utilisateur trouvé avec une expiration de compte dans moins de $Jours jours."
    }
} catch {
    Write-Error "Une erreur s'est produite : $_"
}
