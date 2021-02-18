---
layout: post
title: "Checking if AX column (field) exists on table in SQL"
tags: [AX, Dynamics 365 for Finance and Operations, X++, SQL]
author: Andreas Bjørn Hassing
---

I recently saw use of `DictTable.fieldName2Id` to check if a field exists on a X++ table. If the method returned `0`, it was assumed that the field was not found on the table.

This is, in fairness, a fine approach in most cases. However, for fields that have their field ids hardcoded (system fields, such as `RecId` and `DataAreaId`) the field id will _not_ be `0`.

A robust check if a field exists in the database for a table is to grab the `DictField` for that table and field combination and call `DictField.isSql()` method, which checks to see that the field does in fact exist on the table.

#### `fieldName2Id`-approach

```csharp
fieldExistsOnTableOrIsSystemField("MyTable", "RecId")   // => always true, since `RecId` is a system-field with a hardcoded field id
fieldExistsOnTableOrIsSystemField("MyTable", "MyField") // => true if the field exists on the table in metadata

static boolean fieldExistsOnTableOrIsSystemField(str _tableName, str _fieldName)
{
    DictTable targetDictTable = new DictTable(tableName2Id(_tableName));
    FieldId fieldId = targetDictTable.fieldName2Id(_fieldName);

    return fieldId > 0;
}
```

#### `isSql`-approach

```csharp
fieldExistsOnTable("MyTable", "RecId")   // => true if the field exists on the table in SQL
fieldExistsOnTable("MyTable", "MyField") // => true if the field exists on the table in SQL

static boolean fieldExistsOnTable(str _tableName, str _fieldName)
{
    DictTable targetDictTable = new DictTable(tableName2Id(_tableName));
    FieldId fieldId = targetDictTable.fieldName2Id(_fieldName);
    DictField field = targetDictTable.Fieldobject(fieldId);

    return field.isSql();
}
```
