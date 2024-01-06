
nopus_session:"eyJpdiI6ImxueW9ZWm93MGxJTUErSU5VWnZ5N1E9PSIsInZhbHVlIjoiMnBiWWlnRHdNZU10L1RQaXBBajV1WUViUDJYZExtMTBzUGowUzF1QzlsUDlSaE8yVEtmUStHdzJTMS9oUXViQ1NCRWhZU3ljanRIMHZPczYrU3V1aENUaU9qbWpkKy9yaEQyZVVWSkZiYWpmRU44MXJ4NnVReDZ1QmNpKzBmc0YiLCJtYWMiOiJkZTkwNWVjYzFmYjU3ZWE3ZWQ0ZGFlNDMzOTcwZTVlNzM2MzhlZTdiMjYxZWYyNGYxMTc4MDU1YmJmNTIyMGRkIiwidGFnIjoiIn0%3D"

XSRF-TOKEN:"eyJpdiI6IkFJeTFKYzNuNDlzV1ozM2dPTGdZSXc9PSIsInZhbHVlIjoiUFNyUVVkK2MzOXRGQTBJMmsxQVZTaUZ6d21QWWdaY20zdE1YcUJrZTBJeDA0aWI0Q00xRXdOSnJXZEI0eVZRT0sybXdXTHFKZnZwb2w0QXlKYklVZDFHMzUvTFFQUnNCTUhCTEh5d1hISms2VSszU1FVY1NVdlJMemRaYWtmM2IiLCJtYWMiOiIwZjMxODI4ZGUzYjcyNjE1MTdiZTMyNTlhM2Y3NDMyMjlhZGM3N2I5NmEzZjYzZTMwNTkyNjc0NzkxZmZlOWQ0IiwidGFnIjoiIn0%3D"

Sadrzaj session fajla:
```php
a:4:{s:6:"_token";s:40:"CTSoX2BYVo7cqmNo4KERrLRcepwg20yiF3CofrGQ";s:6:"_flash";a:2:{s:3:"old";a:0:{}s:3:"new";a:0:{}}s:9:"_previous";a:1:{s:3:"url";s:56:"https://nopus.online/spa/get-bills-for-approvement-count";}s:50:"login_web_59ba36addc2b2f9401580f014c7f58ea4e30989d";i:1;}
```

```php
>>> unserialize('a:4:{s:6:"_token";s:40:"CTSoX2BYVo7cqmNo4KERrLRcepwg20yiF3CofrGQ";s:6:"_flash";a:2:{s:3:"old";a:0:{}s:3:"new";a:0:{}}s:9:"_previous";a:1:{s:3:"url";s:56:"https://nopus.online/spa/get-bills-for-approvement-count";}s:50:"login_web_59ba36addc2b2f9401580f014c7f58ea4e30989d";i:1;}')
=> [
     "_token" => "CTSoX2BYVo7cqmNo4KERrLRcepwg20yiF3CofrGQ",
     "_flash" => [
       "old" => [],
       "new" => [],
     ],
     "_previous" => [
       "url" => "https://nopus.online/spa/get-bills-for-approvement-count",
     ],
     "login_web_59ba36addc2b2f9401580f014c7f58ea4e30989d" => 1,  <--- vidi Illuminate\Auth\SessionGuard::getName(), value(1) je id prijavljenog korisnika
   ]
```

Vidi VerifyCsrfToken middleware: Illuminate\Foundation\Http\Middleware\VerifyCsrfToken

CSRF token se generise po inicijalizaciji sesija i takav se upise u session file.
Kad se korisnik odjavi (logout), sessija je se regenerise.
CSRF token se provjerava samo kod !=GET zahtjeva.
CSRF token se iz formi salje kao skriveno input polje _token.
U SPA aplikacijama CSRF token se salje u HTTP Request X-XSRF-TOKEN header-u
Laravel vraca CSRF token enkriptovan i XSRF-TOKEN cookie-u
AXIOS ga automatski uzima iz kukija i sprema u Request header
