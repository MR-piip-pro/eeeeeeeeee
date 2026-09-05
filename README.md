Get-Acl "$env:USERPROFILE\.ssh\authorized_keys" | Format-List

ممتاز، الآن وجدنا المشكلة المحتملة. 👍

الملف موجود، والمفتاح صحيح، لكن ملكية وصلاحيات authorized_keys غير مناسبة لحساب Him Rf؛ الملف مملوك لـ Administrators، وليس لحسابك.

سنصلح صلاحيات الملف فقط، ولا نلمس المفتاح نفسه.

على Windows PowerShell كمسؤول نفّذ هذه الأوامر بالترتيب:

$path = "$env:USERPROFILE\.ssh\authorized_keys"

ثم:

icacls $path /inheritance:r

ثم:

icacls $path /grant:r "$env:USERNAME:F"

ثم:

icacls $path /grant:r "SYSTEM:F"

بعدها جرّب من Termux:

ssh 'Him Rf'@192.168.11.101

النتيجة التي نريدها

إذا كان كل شيء صحيحًا، لن يطلب كلمة المرور، وستدخل مباشرة إلى Windows.

إذا طلب كلمة المرور مرة أخرى، لا تدخلها. أرسل لي فقط ما يظهر، وسنكمل من هناك.

والتحذير:

WARNING: connection is not using a post-quantum key exchange algorithm

يمكن تجاهله حاليًا؛ ليس هو سبب المشكلة.
