AutoIndexer
===========
Android library for generating db indexes.

Basic usage
-----------
Define the relationships between your data models using [Thneed](https://github.com/chalup/thneed) and use this information to generate the proper indexes:
```java
for (SqliteIndex index : AutoIndexer.generateIndexes(MODEL_GRAPH)) {
  db.execSQL(AutoIndexer.getCreateStatement(index));
}
```

Note that this will generate the indexes for both ends of the relationships, which might not be exactly what you want. For example it will generate the index for primary keys referenced from other columns. We provide the `Predicate` factory to filter the generation results:
```java
FluentIterable<SQLiteIndex> indexes = FluentIterable
    .from(AutoIndexer.generateIndexes(MODEL_GRAPH))
    .filter(Predicates.not(isIndexOnColumn(ModelColumns.ID)))
    .filter(Predicates.not(isIndexOnColumn(BaseColumns._ID)));
```

And if you don't want index generation, you can still use our API to get the create index statement (although, I admit, it's not a killer feature):
```java
AutoIndexer.getCreateStatement(new SQLiteIndex("my_table", "foobar_id"));
```

Hint: when you're automagically generating the indexes, you want to automagically clean them up as well. Use [SQLiteMaster](https://github.com/futuresimple/sqlitemaster) utility to do this:
```java
SQLiteMaster.dropIndexes(db);
```

Building
--------
This is standard maven project. To build it just execute:
```shell
mvn clean package
```
in directory with pom.xml.

minSdkVersion = 10
------------------
AutoIndexer is compatible with Android 2.3 and newer.

Todo
----
* Documentation
* Unit tests

License
-------
    Copyright (C) 2013 Jerzy Chalupski

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

         http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. 
