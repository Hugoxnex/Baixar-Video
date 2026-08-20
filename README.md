# Baixar-Video

# Verifica se o yt-dlp ja esta instalado, senao instala via Winget
$ytdlp = Get-Command yt-dlp -ErrorAction SilentlyContinue
if (-not $ytdlp) {
    Write-Host "==================================================="
    Write-Host "  VERIFICANDO DEPENDENCIAS DO SISTEMA"
    Write-Host "==================================================="
    Write-Host "[AVISO] yt-dlp nao foi encontrado. Instalando via Winget..."
    winget install yt-dlp.yt-dlp --silent --accept-source-agreements --accept-package-agreements

    $ytdlp = Get-Command yt-dlp -ErrorAction SilentlyContinue
    if ($ytdlp) {
        Write-Host "[SUCESSO] yt-dlp instalado com exito!"
        Start-Sleep -Seconds 2
    } else {
        Write-Host "[ERRO] Falha ao instalar o yt-dlp automaticamente."
        Write-Host "Tente executar este script como Administrador."
        Read-Host "Pressione Enter para sair"
        exit
    }
} else {
    # Mantem o yt-dlp atualizado, pois o YouTube muda constantemente e causa erros 403 em versoes antigas
    Write-Host "Verificando atualizacoes do yt-dlp..."
    yt-dlp -U
}

# Garante o browser-cookie3 (usado pelo yt-dlp para ler cookies do navegador e evitar bloqueios do YouTube)
$pip = Get-Command pip -ErrorAction SilentlyContinue
if ($pip) {
    $pkgInstalado = pip show browser-cookie3 2>$null | Select-String -Pattern "^Version:\s*(.+)$"
    $precisaInstalar = $true
    if ($pkgInstalado) {
        $versaoAtual = [version]($pkgInstalado.Matches[0].Groups[1].Value -replace '[^\d\.]','')
        if ($versaoAtual -ge [version]"0.20.1") { $precisaInstalar = $false }
    }
    if ($precisaInstalar) {
        Write-Host "Instalando/atualizando browser-cookie3..."
        pip install "browser-cookie3>=0.20.1" --quiet
    }
}

# --- INICIO DA INTERFACE GRAFICA ---
Add-Type -AssemblyName PresentationFramework
Add-Type -AssemblyName System.Windows.Forms

[xml]$XAML = @"
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Baixador do YouTube (yt-dlp)" Height="430" Width="520" WindowStartupLocation="CenterScreen" ResizeMode="NoResize" Background="#F4F6F9">
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <!-- Titulo -->
        <TextBlock Grid.Row="0" Text="Baixador de Midia Avancado" FontSize="20" FontWeight="Bold" Foreground="#CC0000" HorizontalAlignment="Center" Margin="0,0,0,15"/>

        <!-- Campo de Link -->
        <StackPanel Grid.Row="1" Margin="0,0,0,15">
            <TextBlock Text="Insira a URL do Video:" FontWeight="Bold" Margin="0,0,0,5"/>
            <TextBox Name="TxtUrl" Padding="8" FontSize="13" BorderBrush="#CCCCCC" BorderThickness="1"/>
        </StackPanel>

        <!-- Selecao de Formato -->
        <GroupBox Grid.Row="2" Header="Selecione o Formato de Download" FontWeight="Bold" Padding="10" Margin="0,0,0,15" Background="White">
            <StackPanel>
                <RadioButton Name="RadVideo" Content="Video Completo (MP4)" IsChecked="True" Margin="0,5" FontWeight="Normal"/>
                <RadioButton Name="RadMp3" Content="Apenas Audio (Alta Qualidade MP3)" Margin="0,5" FontWeight="Normal"/>
                <RadioButton Name="RadM4a" Content="Apenas Audio (Formato M4A padrao)" Margin="0,5" FontWeight="Normal"/>
            </StackPanel>
        </GroupBox>

        <!-- Botoes -->
        <StackPanel Grid.Row="3">
            <Button Name="BtnBaixar" Content="Baixar Agora" Background="#CC0000" Foreground="White" FontWeight="Bold" Padding="10" FontSize="14" Cursor="Hand"/>
            <Button Name="BtnPasta" Content="Abrir Pasta dos Videos" Background="#2B78E4" Foreground="White" FontWeight="Bold" Padding="8" FontSize="12" Margin="0,10,0,0" Cursor="Hand"/>
        </StackPanel>
    </Grid>
</Window>
"@

$reader = (New-Object System.Xml.XmlNodeReader $XAML)
$Form = [Windows.Markup.XamlReader]::Load($reader)

$TxtUrl = $Form.FindName("TxtUrl")
$RadVideo = $Form.FindName("RadVideo")
$RadMp3 = $Form.FindName("RadMp3")
$RadM4a = $Form.FindName("RadM4a")
$BtnBaixar = $Form.FindName("BtnBaixar")
$BtnPasta = $Form.FindName("BtnPasta")

$BtnBaixar.Add_Click({
    $url = $TxtUrl.Text.Trim()
    if ([string]::IsNullOrEmpty($url)) {
        [System.Windows.Forms.MessageBox]::Show("Por favor, cole uma URL valida do YouTube!", "Aviso", [System.Windows.Forms.MessageBoxButtons]::OK, [System.Windows.Forms.MessageBoxIcon]::Warning)
        return
    }

    # Usa cookies do Firefox para evitar bloqueios do YouTube (nao precisa fechar nenhum navegador)
    $cookies = '--cookies-from-browser firefox'

    if ($RadVideo.IsChecked) {
        $formato = '-f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]"'
        $titulo = "Baixando Video"
    }
    elseif ($RadMp3.IsChecked) {
        $formato = '-x --audio-format mp3 --audio-quality 0'
        $titulo = "Baixando Audio MP3"
    }
    elseif ($RadM4a.IsChecked) {
        $formato = '-x --audio-format m4a'
        $titulo = "Baixando Audio M4A"
    }

    # Se os cookies falharem (ex: Firefox nao instalado), tenta de novo sem cookies
    $cmdComCookies = "yt-dlp $cookies $formato `"$url`""
    $cmdSemCookies = "yt-dlp $formato `"$url`""
    Start-Process cmd.exe -ArgumentList "/c title $titulo && echo Iniciando download... && ($cmdComCookies || (echo [AVISO] Falha com cookies, tentando sem cookies... && $cmdSemCookies)) || pause" -Wait
})

$BtnPasta.Add_Click({
    Start-Process explorer.exe -ArgumentList "."
})

$Form.ShowDialog() | Out-Null
