### Header Injection

#### header_crlf_basic.txt
Нагрузки для проверки **CRLF-инъекций в HTTP-заголовки** без подмены тела ответа.  
Содержит варианты с `%0d%0a`, `%0a`, `%0d`, которые добавляют свои заголовки (`X-Injected-Header`, `Set-Cookie`, `Location`, `Refresh`).  
Подходит для безопасного тестирования на боевых средах (не ломает тело ответа намеренно).

#### header_crlf_body.txt
Агрессивные CRLF-пейлоады для **response splitting / подмены тела ответа**.  
Используют `Content-Type`, `Content-Length` и вставку собственного HTML-контента после дополнительной пары CRLF.  
Рекомендуется использовать **только на тестовых стендах**, так как может ломать формат ответа и кэш.

#### header_host_external.txt
Нагрузки для манипуляции заголовками **Host / X-Forwarded-Host / Origin / Referer** внешними доменами.  
Содержит разные варианты атакующих хостов и поддоменов (`attacker.example.com`, `evil.com`, `sub.attacker.example.com` и т.п.).  
Используется для обнаружения уязвимостей, связанных с:
- SSRF через Host/Origin,
- неверной обработкой хоста при генерации ссылок и redirect’ов,
- некорректной валидацией внешних доменов.

#### header_host_internal.txt
Нагрузки для имитации **внутренних/локальных хостов** в заголовках Host / Origin / X-Forwarded-Host.  
Содержит `localhost`, `127.0.0.1`, `[::1]`, а также псевдо-внутренние домены (`internal.example.local`, `intranet.local` и др.).  
Используется для проверки SSRF и логики, завязанной на «доверенные» внутренние хосты.

#### header_host_advanced.txt
Продвинутые нагрузки для Host / X-Forwarded-Host:
- обфускация (`victim.example.com.evil.com`, `example.com@evil.com`, `evil.com#victim.example.com`),
- URL-энкодинг (`evil.com%2Fvictim.example.com`, `evil.com%23victim.example.com`),
- комбинирование Host с CRLF (`%0d%0aX-Injected-Host: 1`).  

Используется для обхода наивных проверок домена и детекта сложных кейсов:
- host-based auth/bypass,
- cache poisoning,
- комбинированных Host + CRLF атак.

