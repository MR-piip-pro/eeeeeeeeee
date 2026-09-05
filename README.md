icacls "$env:USERPROFILE\.ssh\authorized_keys" /grant "Him Rf:F"


Get-Content "C:\ProgramData\ssh\sshd_config" | Select-String "AuthorizedKeysFile|Match"
