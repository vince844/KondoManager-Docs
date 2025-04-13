---
description: Guida alla gestione dei permessi lato frontend
---

# Gestione permessi frontend

La **usePermission composable** è altamente flessibile e permette di gestire sia i ruoli che i permessi. Permette di controllare l'accesso in base a una combinazione di entrambi oppure per singoli ruoli o permessi&#x20;

### Come funziona la usePermission

* **hasRole(roles)**: Controlla se l'utente ha almeno un ruolo di quelli specificati nell'array.
* **hasPermission(permissions)**: Controlla se l'utente ha almeno un permesso di quelli specificati nell'array.
* **canAccess(item)**: La funzione più flessibile:
  * Se si pass un array di permessi **permissions array** (`string[]`), la funzione controlla se l'utente ha uno di questi permessi.
  * Se si passa un **NavItem** oppure un semplice object con `role?` e `permissions?`, la funzione controlla se i ruoli e i permessi sono presenti.
  * Se non vienne passato nessun `role` o `permissions` l'accesso è garantito di default.

### Come utilizzare la usePermission

&#x20;Per poter utilizzare la  usePermission composable è necessario per prima cosa importarla

```
import { usePermission } from "@/composables/permissions";
```

Successivamente è possibile utilizzare le seguenti funzioni:

```
const { hasRole } = usePermission();


```
