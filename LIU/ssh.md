# SSH
För att ansluta till LIU servrar behöver man antingen vara ansluten till LiU:s nätverk eller använda deras VPN.
Detta ska gå att kringå genom att ssh'a till lysators servrar och sedan ssh'a vidare då men hamnar på rätt nätverk.
## Ansluta till LIU:s servrar
``` ssh -X abcde123@ssh.edu.liu.se ```

Byt ut abcde123 mot ditt LiU-konto. -X flaggan gör att du kan köra grafiska program på servern och få dem att visas på din dator.
## Ansluta till Muxen salar
```ssh -X abcde123@muxenX-1YY.ad.liu.se```

Byt ut abcde123 mot ditt LiU-konto, X med det nummer som motsvarar din salen och YY med det nummer som motsvarar datorn. Salar 1-4 och datorer 101-116.