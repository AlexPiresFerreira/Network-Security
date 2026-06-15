
=========================================================
Report
select *, from $log
select action,srcip,dstip,url,profile,hostname from $log
select action,srcip,dstip,url,profile,hostname from $log where hostname='netflix.com'
(traffic) select dstip as ip_destino, count(*) as sesiones from $log where $filter and dstip is not null group by dstip order by sesiones desc limit 10 offset 1

=========================================================
Migrar S/N da caixa
execute migrate serial-number-list <Serial atual>
Verificar: diagnose test application oftpd 3