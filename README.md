icacls "$env:USERPROFILE\.ssh\authorized_keys" /grant "Him Rf:F"


Get-Content "C:\ProgramData\ssh\sshd_config" | Select-String "AuthorizedKeysFile|Match"

Get-LocalGroupMember -Group "Administrators"



نعم، الآن تأكد السبب. ✅

حساب Him Rf عضو في مجموعة Administrators، ولذلك Windows OpenSSH لا يستخدم:

C:\Users\Him Rf\.ssh\authorized_keys

بل يستخدم ملف المسؤولين:

C:\ProgramData\ssh\administrators_authorized_keys

ولهذا كان الهاتف يرسل المفتاح الصحيح، لكن Windows يرفضه.Test-Path "C:\ProgramData\ssh\administrators_authorized_keys"

نصلحها الآن

على PowerShell كمسؤول نفّذ:

Copy-Item "$env:USERPROFILE\.ssh\authorized_keys" "C:\ProgramData\ssh\administrators_authorized_keys" -Force

ثم اضبط صلاحيات الملف:

icacls "C:\ProgramData\ssh\administrators_authorized_keys" /inheritance:r

ثم:

icacls "C:\ProgramData\ssh\administrators_authorized_keys" /grant "Administrators:F"

ثم:

icacls "C:\ProgramData\ssh\administrators_authorized_keys" /grant "SYSTEM:F"

بعد ذلك لا تحتاج إلى إعادة تشغيل الكمبيوتر.

اذهب إلى Termux وجرب:

ssh 'Him Rf'@192.168.11.101

النتيجة المطلوبة

إذا نجح كل شيء، سيدخل SSH مباشرة بدون طلب كلمة المرور. 🎯

إذا طلب كلمة المرور مرة أخرى، لا تدخلها؛ أرسل لي فقط ما يظهر، وسنكمل من النقطة نفسها.
Test-Path "C:\ProgramData\ssh\administrators_authorized_keys"

ا 


Test-Path "C:\ProgramData\ssh\administrators_authorized_keys"
