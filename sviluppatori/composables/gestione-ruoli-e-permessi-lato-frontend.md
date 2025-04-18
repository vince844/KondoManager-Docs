---
description: Guida alla gestione dei ruoli e permessi utente lato frontend
icon: key
---

# Gestione ruoli e permessi lato frontend

La **usePermission composable** è altamente flessibile e permette di gestire sia i ruoli che i permessi. Permette di controllare l'accesso agli elemneti del frontend in base a una combinazione di entrambi oppure per singoli ruoli o permessi&#x20;

### Come funziona la usePermission

* **`hasRole(roles: string[])`**\
  Controlla se l'utente ha almeno **uno** dei ruoli specificati.
* **`hasPermission(permissions: string[])`**\
  Controlla se l'utente ha almeno **uno** dei permessi specificati.
* **`canAccess(item)`**\
  La funzione più flessibile:
  * Se riceve un **array di permessi** (`string[]`), verifica se l'utente ha almeno uno di quei permessi.
  * Se riceve un **oggetto con `role?` e/o `permissions?`**, verifica se almeno uno dei ruoli o permessi è presente.
  * Se **non vengono passati né ruoli né permessi**, l'accesso viene **garantito** di default.

### Come utilizzare la usePermission

&#x20;Per poter utilizzare la  usePermission composable è necessario per prima cosa importarla

```
import { usePermission } from "@/composables/permissions";
```

#### Funzione hasRole per controllare uno o più ruoli

Se si vuole dare accesso ad un elemento del frontend in base a uno o più ruoli. La funzione hasRole accetta un array di ruoli es \[ruolo1, ruolo2, ...]

```
const { hasRole } = usePermission();

if (hasRole(['amministratore'])) {
  console.log('Salve amministratore');
}
```

Mostra un elemento se l'utente ha un ruolo

```
<div v-if="hasRole(['amministratore'])">
  Salve amministratore
</div>
```

#### Funzione hasPermission per controllare uno o più permessi

Se si vuole dare accesso ad un elemento del frontend in base a uno o più permessi. La funzione hasPermisssion accetta un array di permessi es \[permesso1, permesso2, ...]

```
const { hasPermission } = usePermission();

if (hasPermission(['modifica utenti'])) {
  console.log('Questo utente ha permessi per modificare gli utenti');
}
```

Mostra un elemento se l'utente ha un permesso

```
<div v-if="hasPermission(['modifica utenti'])">
  Puoi modificare gli utenti
</div>
```

#### Funzione canAccess per controllare sia ruolo che permessi

Se si vuole controllare sia la presenza di uno o più ruoli che la presenza di uno o più permessi è possibile utilizzare la funzione canAccess

```
const { canAccess } = usePermission();

if (canAccess({ roles: ['amministratore'], permissions: ['modifica utenti'] })) {
  console.log('Questo utente può essere un amministratore oppure avere permessi per modificare gli utenti');
}
```

Mostra un elemento se l'utente ha un ruolo o più di uno oppure un permesso o più d uno

```
<div v-if="canAccess({ roles: ['amministratore'], permissions: ['elimina utente'] })">
  Puoi accedere a questo elemento
</div>
```

#### Utilizzo all'interno di un menù di navigazione

È possibile utilizzare la funzione canAccess per impostare i permessi di visualizzazione degli elementi del menù

```
const items: NavItem[] = [
  { title: 'Dashboard', href: '/dashboard', roles: ['amministratore'] },
  { title: 'Segnalazioni', href: '/utenti', permissions: ['visualizza utenti'] },
];

<NavigationMenuItem
  v-for="item in items"
  :key="item.href"
  v-if="canAccess(item)"
>
  <Link :href="item.href">{{ item.title }}</Link>
</NavigationMenuItem>
```

#### Computed conditions

```
const canSeeDeleteButton = computed(() =>
  canAccess({ permissions: ['Elimina segnalazioni'] })
);

const isAdminOrCanEdit = computed(() =>
  canAccess({ roles: ['amministratore'], permissions: ['Modifica segnalazioni'] })
);
```

#### Suggerimenti sull'utilizzo

La gestione della visualizza degli elementi lato frontend tramite l'utilizzo di ruoli e permessi si deve solo utilizzare per gestire gli elementi di layout, è sempre eseguire dei controlli anche dal alto backend utilizzando le policies di Lararavel per garantire una sicura gestione dei ruoli e permessi

* Utilizza `hasRole()` quando vuoi esplicitamente controlalre il ruolo dell'utente&#x20;
* Utilizza `hasPermission()` per specifiche azioni che l'utente può compiere
* Utilizza `canAccess()` quando la logia involve uno o entramabi ruoli o permessi, oppure quando è necessario avere un controllo dinamico sugli elementi di un menù
