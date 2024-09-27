# Charger l'assembly nécessaire pour les formulaires Windows
Add-Type -AssemblyName System.Windows.Forms

# Variable pour le job en cours
$currentJob = $null

# Fonction pour arrêter un job en cours
function StopCurrentJob {
    if ($currentJob -ne $null -and $currentJob.State -eq 'Running') {
        Stop-Job -Job $currentJob
        Remove-Job -Job $currentJob
        $currentJob = $null
        [System.Windows.Forms.MessageBox]::Show("Blague arrêtée.")
    }
}

# Créer la fenêtre principale
$form = New-Object System.Windows.Forms.Form
$form.Text = "Blagues Inoffensives"
$form.Size = New-Object System.Drawing.Size(400, 550)

# Ajouter un label
$label = New-Object System.Windows.Forms.Label
$label.Text = "Choisis une blague à faire :"
$label.Size = New-Object System.Drawing.Size(300, 20)
$label.Location = New-Object System.Drawing.Point(50, 20)
$form.Controls.Add($label)

# Fonctions de blagues

# Blague 1 - Fenêtre de message infinie
function Blague1 {
    while ($true) {
        [System.Windows.Forms.MessageBox]::Show("Ton ordinateur a rencontré un problème... Mais non, c'est juste une blague !", "Erreur Critique")
        Start-Sleep -Seconds 3
    }
}

# Blague 2 - Simulation de frappe aléatoire
function Blague2 {
    $random = New-Object System.Random
    while ($true) {
        Start-Sleep -Seconds ($random.Next(3, 10))
        [System.Windows.Forms.SendKeys]::SendWait("{ENTER}")
        [System.Windows.Forms.SendKeys]::SendWait("C'est une blague !")
    }
}

# Blague 3 - Ouvrir des applications aléatoires
function Blague3 {
    $apps = @("calc", "notepad", "mspaint")
    $random = New-Object System.Random
    while ($true) {
        Start-Sleep -Seconds ($random.Next(5, 10))
        Start-Process $apps[$random.Next(0, $apps.Length)]
    }
}

# Blague 4 - Inversion aléatoire de l'écran
function Blague4 {
    while ($true) {
        Start-Sleep -Seconds 5
        (New-Object -ComObject WScript.Shell).SendKeys("^%{DOWN}")  # Inverse l'écran
        Start-Sleep -Seconds 3
        (New-Object -ComObject WScript.Shell).SendKeys("^%{UP}")  # Remet l'écran normal
    }
}

# Blague 5 - Afficher l'heure aléatoire
function Blague5 {
    while ($true) {
        Start-Sleep -Seconds 5
        [System.Windows.Forms.MessageBox]::Show("Il est " + (Get-Date).ToString(), "Quelle heure est-il ?")
    }
}

# Blague 6 - Ouvrir le CD-ROM (si possible)
function Blague6 {
    while ($true) {
        Start-Sleep -Seconds 10
        (New-Object -ComObject WMPlayer.OCX).cdromCollection.Item(0).Eject()
    }
}

# Blague 7 - Éteindre l'écran pendant 5 secondes
function Blague7 {
    while ($true) {
        Start-Sleep -Seconds 15
        (New-Object -ComObject WScript.Shell).SendKeys("^%{ESC}")
        Start-Sleep -Seconds 5
        (New-Object -ComObject WScript.Shell).SendKeys("^%{ESC}")
    }
}

# Blague 8 - Déplacer la souris aléatoirement
function Blague8 {
    Add-Type -AssemblyName System.Windows.Forms
    $random = New-Object System.Random
    while ($true) {
        Start-Sleep -Seconds ($random.Next(2, 5))
        [System.Windows.Forms.Cursor]::Position = New-Object System.Drawing.Point($random.Next(0, 800), $random.Next(0, 600))
    }
}

# Blague 9 - Jouer un son d'erreur Windows
function Blague9 {
    while ($true) {
        Start-Sleep -Seconds 5
        [System.Media.SystemSounds]::Hand.Play()  # Son d'erreur Windows
    }
}

# Blague 10 - Activer/Désactiver le verrouillage du clavier
function Blague10 {
    while ($true) {
        Start-Sleep -Seconds 7
        [System.Windows.Forms.SendKeys]::SendWait("{NUMLOCK}")
        Start-Sleep -Seconds 3
        [System.Windows.Forms.SendKeys]::SendWait("{NUMLOCK}")
    }
}

# Ajouter les boutons pour chaque blague
$blagues = @(
    @{Text="Blague 1 : Fenêtre de message"; Action={Blague1}},
    @{Text="Blague 2 : Frappe aléatoire"; Action={Blague2}},
    @{Text="Blague 3 : Applications aléatoires"; Action={Blague3}},
    @{Text="Blague 4 : Inversion écran"; Action={Blague4}},
    @{Text="Blague 5 : Afficher l'heure"; Action={Blague5}},
    @{Text="Blague 6 : Ouvrir CD-ROM"; Action={Blague6}},
    @{Text="Blague 7 : Éteindre l'écran"; Action={Blague7}},
    @{Text="Blague 8 : Déplacer la souris"; Action={Blague8}},
    @{Text="Blague 9 : Son d'erreur Windows"; Action={Blague9}},
    @{Text="Blague 10 : Verrouillage clavier"; Action={Blague10}}
)

# Positionnement dynamique des boutons
$yPosition = 50  # Position verticale initiale pour les boutons
foreach ($blague in $blagues) {
    $button = New-Object System.Windows.Forms.Button
    $button.Text = $blague.Text
    $button.Size = New-Object System.Drawing.Size(300, 30)
    $button.Location = New-Object System.Drawing.Point(50, $yPosition)
    $button.Add_Click({
        StopCurrentJob
        $currentJob = Start-Job $blague.Action
    })
    $form.Controls.Add($button)
    $yPosition += 40  # Incrémenter la position pour le prochain bouton
}

# Bouton pour arrêter la blague en cours
$buttonStop = New-Object System.Windows.Forms.Button
$buttonStop.Text = "Arrêter la blague"
$buttonStop.Size = New-Object System.Drawing.Size(300, 30)
$buttonStop.Location = New-Object System.Drawing.Point(50, $yPosition)
$buttonStop.Add_Click({
    StopCurrentJob
})
$form.Controls.Add($buttonStop)

# Afficher la fenêtre
$form.ShowDialog()
