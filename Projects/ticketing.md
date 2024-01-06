
# SIMPLE TICKETING - opis projekta

Namjena aplikacije je da kompanijama, koje su pruzaoci neke usluge, pojednostave upravljanje poslom.

## Primjeri ticketing sistema, identifikacija mogucih funkcionalnosti
- [osticket](https://osticket.com/features/)
- [laradesk](https://dacoto.com/products/laradesk/demo)
- [handesk](http://handesk.local/login)
- [mojohelpdesk](https://mojohelpdesk.com/features/)
- [neke dobre ilustracije](https://www.spiceworks.com/free-help-desk-software/)
- [images](https://www.google.com/search?q=ticketing+system+free&client=ubuntu&hs=9xa&channel=fs&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjJ94Kgw5X8AhWih_0HHTxAA30Q_AUoAXoECAEQAw&biw=1920&bih=939&dpr=1#imgrc=oRI8vbWdbOhF2M)

## Definicija zahtjeva

# KORISNICKE ROLE:
[Spatie Model Permissions](https://spatie.be/docs/laravel-permission/v5/introduction)

Osoba koja ce administrirati agente i korisnike, konfigurirati nivoe prioriteta i kategorije, ima rolu ADMIN.
Obicno u kompaniji koja pruza uslugu imamo osobu koja prihvata tikete i dodjeljuje ih dalje na rjesavanje, za tu ulogu kreiramo rolu DISPATCHER.
DISPATCHER je odgovoran za tikete, on sprovodi dogovoreni SLA.
Osoba koja je zaduzena za rjesavanje tiketa ima rolu AGENT.

Tiket moze otvoriti i DISPATCHER, u ime klijenta(requester), ako mu poziv stigao na email ili nekim drugim kanalom.

Tiket ce imati popunjeno polje za source (app - iz aplikacije, phone, email, other).

# DEFINICIJA STATUSA I PROMJENE:
[Spatie Model States](https://spatie.be/docs/laravel-model-states/v2/01-introduction)

Samo DISPATCHER moze otvoriti novi ticket, tada ticket iz status NEW prelazi u status OPEN.
DISPATCHER dodjeljuje ticket na rjesavanje AGENTU.
Samo AGENT moze iz statusa OPEN pokrenuti rjesavanje tiketa, tj prebaciti tiket iz status OPEN u PENDING.
Samo AGENT kome je dodjeljen ticket moze prebaciti ticket iz statusa PENDING U SOLVED.
Samo DISPATCHER i USER moze prebaciti ticket iz statusa SOLVED u CLOSED.
Samo DISPATCHER (i aplikacija automatski) moze prebaciti ticket u status ARCHIVED.

STATUSI: NEW, OPEN, PENDING, SOLVED, CLOSED
status2: unassigned, assigned // odredjuje ga polje assigned_to

# TICKET LOG
Tiket MORA imati vezanu istoriju, ticket log, za svaki dogadjaj tj promjenu statusa.
Iz ovoga se pravi osnova za racunanje KPI, a imamo i odgovarajuci audit.

# FRONTEND

## DISPATCHER screen
New ticket button +

Tickets
 - My tickets / tiketi dodjeljeni
 - Open tickets / unutar ce biti filtri za new|open(ima tag unassigned, assigned, wait_for_resolving ili pending)||unassigned|pending|solved
 - closed
 - archived


Iz paketa [*coderflexx/laravel-ticket*](https://github.com/coderflexx/laravel-ticket)

```php
<?php

namespace Coderflex\LaravelTicket\Enums;

enum Status: string
{
    case OPEN = 'open';
    case CLOSED = 'closed';
    case ARCHIVED = 'archived';
}
```

```php
<?php

namespace Coderflex\LaravelTicket\Enums;

enum Priority: string
{
    case LOW = 'low';
    case NORMAL = 'normal';
    case HIGH = 'high';
}
```

# PRICING

Orjentaciono, za implementaciju 2000e do 4000e.
Ovo podrazumjeva osnovne funkcionalnosti.
Aplikacija ce imati administrator rolu, dispatcher, agent rolu i user rolu.
Administrator je iz firme koja pruza servis, tj koja prima tikete.
On kreira agente - to su ljudi iz iste firme kojima se dodjeljuju tiketi i koji ih rjesavaju.
Useri su iz firme kojima se pruza usluga, oni otvaraju tikete.
Admin ce moci da dodaje klijente, tj firme kojima pruza usluge.
Tiketi ce imati odredjene statuse, nivoe prioriteta i odgovarajuce kategorije.
Ovo ce biti konfigurabilno.
Postojace i neke email notifikacije.
Korisnicki interface ce biti prilagodjen i za telefone.
To je ukratko.

Cijena moze da varira zavisno od funkcionalnost. Ako budu zainteresovani, spremit cu im detaljnu listu za iducu sedmicu.

Odrzavanje prva 3 mjeseca besplatno, to je neki garantni rok za ispravke uocenih nepravilnosti.

Ako ova tvoja firma bude htjela raditi aplikaciju, za Srdju onda implementacija moze biti 50% jeftinija.

Aplikacija ce biti instalirana na virtuelnom serveru u Digitalocean cloud-u.

Odrzavanje koje podrazumjeva redovni backup, potrebne upgrade operativnog sistema i svih drugih vezanih aplikacije, sigurnosne postavke, ispravka bug-ove. Sve u svemu odrzavanje dostupnosti aplikacije na najvisem nivou ce biti 150Eur mjesecno.
