name: amazonRDP

on: workflow_dispatch

jobs:
  build:

    runs-on: windows-latest
    timeout-minutes: 9999

    steps:
    - name: Downloading Ngrok.
      run: |
        Invoke-WebRequest https://raw.githubusercontent.com/romain09/AWS-RDP/main/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip
        Invoke-WebRequest https://raw.githubusercontent.com/romain09/AWS-RDP/main/start.bat -OutFile start.bat
    - name: Extracting Ngrok Files.
      run: Expand-Archive ngrok.zip
    - name: Connecting to your Ngrok account.
      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
    - name: Activating RDP access.
      run: | 
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
        Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
    - name: Creating Tunnel.
      run: Start-Process Powershell -ArgumentList '-Noexit -Command ".\ngrok\ngrok.exe tcp 3389"'
    - name: Connecting to your RDP.
      run: cmd /c start.bat
    - name: RDP is ready!
      run: | 
        Invoke-WebRequest https://raw.githubusercontent.com/romain09/AWS-RDP/main/loop.ps1 -OutFile loop.ps1
        ./loop.ps1
