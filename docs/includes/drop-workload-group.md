Löscht eine vorhandene benutzerdefinierte Arbeitsauslastungsgruppe der Ressourcenkontrolle.

:::image type="icon" source="../database-engine/configure-windows/media/topic-link.gif"::: [Transact-SQL-Syntaxkonventionen](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
DROP WORKLOAD GROUP group_name
[;]
```

## <a name="arguments"></a>Argumente

*group_name*: Der Name einer vorhandenen benutzerdefinierten Arbeitsauslastungsgruppe.

## <a name="remarks"></a>Bemerkungen

Die DROP WORKLOAD GROUP-Anweisung ist für die internen oder Standard-Gruppen der Ressourcenkontrolle nicht zulässig.

Sie sollten bei der Ausführung von DDL-Anweisungen mit dem Status der Ressourcenkontrolle vertraut sein. Weitere Informationen finden Sie unter [Resource Governor](../relational-databases/resource-governor/resource-governor.md).

Falls eine Arbeitsauslastungsgruppe aktive Sitzungen enthält, tritt beim Löschen bzw. Verschieben der Arbeitsauslastungsgruppe in einen anderen Ressourcenpool ein Fehler auf, wenn Sie die ALTER RESOURCE GOVERNOR RECONFIGURE-Anweisung aufrufen, um die Änderung zu übernehmen. Führen Sie eine der folgenden Aktionen aus, um dieses Problem zu umgehen:

- Warten Sie, bis die Verbindungen für alle Sitzungen der entsprechenden Gruppe geschlossen wurden, und führen Sie dann die ALTER RESOURCE GOVERNOR RECONFIGURE-Anweisung noch einmal aus.

- Stoppen Sie die Sitzungen in der betreffenden Gruppe explizit mit dem KILL-Befehl, und führen Sie die ALTER RESOURCE GOVERNOR RECONFIGURE-Anweisung noch einmal aus.

- Starten Sie den Server neu. Nach Abschluss des Neustarts wird die gelöschte Gruppe nicht erstellt und die neue Ressourcenpoolzuordnung wird von einer verschobenen Gruppe verwendet.

- Falls Sie nach Ausgabe der DROP WORKLOAD GROUP-Anweisung beschließen, dass Sie keine Sitzungen explizit stoppen möchten, um die Änderung zu übernehmen, können Sie die Gruppe mit dem gleichen Namen, den sie vor Ausgabe der DROP-Anweisung hatte, neu erstellen und dann in den ursprünglichen Ressourcenpool verschieben. Um die Änderungen zu übernehmen, führen Sie die ALTER RESOURCE GOVERNOR RECONFIGURE-Anweisung aus.

## <a name="permissions"></a>Berechtigungen

Erfordert die CONTROL SERVER-Berechtigung.

## <a name="examples"></a>Beispiele

Das folgende Beispiel löscht die Arbeitsauslastungsgruppe namens `adhoc`.

```
DROP WORKLOAD GROUP adhoc;
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Ressourcenkontrolle](../relational-databases/resource-governor/resource-governor.md)
- [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-workload-group-transact-sql.md)  
- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-workload-group-transact-sql.md)
- [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)  