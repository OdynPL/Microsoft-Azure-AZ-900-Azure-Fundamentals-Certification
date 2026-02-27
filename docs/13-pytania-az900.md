<a id="sec-13-pytania-az900"></a>
## 13. Podchwytliwe pytania AZ‑900

[ Powrót do spisu treści](../README.md)

| Pytanie | Odpowiedź |
|---|---|
| Czy subskrypcja ma „swój” koszt? | **Nie.** Płacisz za zasoby; subskrypcja to granica rozliczeń i uprawnień. |
| Czy Region = Availability Zone? | **Nie.** Region to zestaw DC; AZ to separowane DC w regionie. |
| Czy Resource Group musi być w tym samym regionie co zasoby? | **Nie.** Lokalizacja RG to metadane – zasoby mogą być w innych regionach (z wyjątkami usług). |
| Czy RBAC działa „w górę” hierarchii? | **Nie.** Uprawnienia dziedziczą **w dół** (MG → Sub → RG → Resource). |
| Czy Bastion wymaga publicznego IP VM? | **Nie.** Działa przez portal bez publicznego IP. |
| Czy Azure Policy zawsze blokuje wdrożenie? | **Nie.** Zależy od **Effect** (np. Audit nie blokuje). |
| Czy Availability Set = Availability Zone? | **Nie.** Set to logiczne FD/UD; AZ to fizyczna separacja DC. |
| Czy outbound (egress) ruch sieci jest darmowy? | **Nie.** Egress jest rozliczany. |

---
---

[Poprzednia strona](./12-extended-services.md) | [Spis treści](../README.md) | [Następna strona](./14-glosariusz.md)
