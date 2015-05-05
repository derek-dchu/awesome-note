# Hibernate
* Advantages:
  1. Fast performance
  2. Database Independent query
  3. Automatic table creation
  4. Simplifies complex join
  5. Provides query statistics and database status

### Structure
```
				Session Factory 	Connection Provider (optional)
  				     |open				     |
	  Persistent       |                         |
	    Object		 |   begin			     |JNDI
App --------------> Session ---> Transaction -> JDBC -> Database
								   	|		          JTA
						Transaction Provider (optional)
```

#### SessionFactory
* A factory of session
* A client of `ConnectionProvider`
* Holds second level cache

#### Session
* A short-lived object that wraps the JDBC connection.
* Provides interface between app and database
* A factory of `Transaction`, `Query`, `Criteria`.
* Holds first-level cache (mandatory)

#### Transaction
* Atomic unit of work

#### ConnectionProvider (optional)
* A factory of JDBC connections.

#### TranscationFactory (optional)
* A factory of Transaction.

### Hibernate Configuration
hiberate.cfg.xml

```xml
<?xml version='1.0' encoding='UTF-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory>
		<property name="configurations">value</property>

		<mapping resource="class_name.hbm.xml" />
	</session-factory>
</hibernate-configuration>

```

### Persistent Class
#### Plain Old Java Object(POJO) programming model
* Default model.  
* Also named: entity.  

  1. A no-arg constructor with at least package visibility. (reflection.newInstance)
  2. An identifier property
  3. Prefer non-final class. (proxy pattern: lazy loading)(semi-optional)
  4. Declare getter and setter methods (optional)
  5. Implementing `equals()` and `hashCode()`: make sure both of them guarantee that same row of database is mapping to the same object. (note: don't use identifier value)


### Mapping Configuration
class_name.hbm.xml

```xml
<?xml version='1.0' encoding='UTF-8'?>
<!DOCTYPE hibernate-mapping PUBLIC
	"-//Hibernate/Hibernate Mapping DTD 3.0//EN"
	"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
	<class name="Class" table="table_name">
	<id name="id_field" column="column_name"></id>

	<property name="field" column="column_name"></property>
</hibernate-mapping>
```

**note:** table, column can be omitted if class name is same as table name, field name is same as column name.

### Caching
Try to reuse the existing object in the session, in order to reduce the number of connection with database which improves the performance.

* Each bean must has an ID for caching.

**note:** Each level of Caching can only store objects with unique ID.

#### First Level Caching (transaction-level)
Caching on Session: cache within current session.

* Enable by default.
* Each object has three status in a session:
  1. Transient:
    1. instantiate using `new`
    2. no persistent representation in the database and no identifier value has been assigned.
    3. will be destroyed by GC.
  2. Persistent:
    1. has persistent representation in the database and an identifier value.
    2. final state will be synchronized with database when the unit of work completes.
  3. Detached:
    1. has been persistent, but the session is closed.
    2. can reattached to a new session, and all its modification will be persistent again.

```java
A a = new A(); // Transient (map doesn't has a's id)

map.put(id, a) // Persistent

map.put(id, null) // Detached (map still has a's id)

map.put(id, a) // Persistent

load(a): 
	if map.contains(id):
		return map.get(id)
	else:
		return map.put(id, a)
```

* `session.contains(objects)` determine if an instance belongs to the session cache.
* `session.evict(object)` changes the object from persistent to detached.
* `session.clear()` evict all objects.
* `session.merge(object)` changes the object from detached to persistent of current session.
* Change non-id fields of persistent object, database will be updated after the transaction.
* Change id of persistent object, database will insert an new record with that id after the transaction.

#### Second-level Caching (SessionFactory-level/JVM-level)
Caching on SessionFactory: shallow copies will be shared across different sessions (entire application).

* Need to be enabled explicitly.
* Strategies:
  1. read-only: caching will work for read only operation.
  2. nonstrict-read-write: caching will work for read and write but one at a time.
  3. read-write: caching will work for read and write, can be used simultaneously.
  4. transactional: caching will work for transaction.
* `sessionFactory.evict(.class, id)` evict a particular object of class with id.
* `sessionFactory.evict(.class)` evict all objects of class.
* `sessionFactory.evictCollection(.class[, id])` collection version of evict.

##### Configuration
1. CacheProvider
hibernate.cfg.xml

```xml
<!-- set to true by default -->
<property name="cache.use_second_level_cache">true</property>

<property name="cache.provider_class">EH Cache, Swarm, OS, JBoss</property>
```

| Provider | read-only | nonstrict-read-write | read-write| transactional |  
|----------|-----------|----------------------|------------|---------------|  
| Hashtable | Y | Y | Y | N |  
| EH Cache | Y | Y | Y | Y |  
| Swarm Cache | Y | Y | N | N |  
| OS Cache | Y | Y | Y | N |  
| JBoss Cache | Y | N | N | Y |  

2. Add cache usage to hbm file
```xml
<class>
	<cache usage="read-only"/>
	...
</class>
```

3. Configure Cache Provider
* EhCache
```xml
<!-- ehcache.xml -->
<?xml version="1.0"?>  
<ehcache>  
  
<defaultCache   
	maxElementsInMemory="100" 
	eternal="false" 
	timeToIdleSeconds="120"  
	timeToLiveSeconds="200" />  
  
<cache
	name="bean_name" 
	maxElementsInMemory="100" 
	eternal="false" 
	timeToIdleSeconds="5" 
	timeToLiveSeconds="200" /> 
</ehcache>
```

#### The Query Cache
Cache query result sets for the queries with same parameters.

* query cache should always be used in conjunction with the second-level cache because it caches only identifier values and results of value type.

##### Configure
By default, both the entire query cache and the cache for individual query are disabled, because most of queries do not benefit from it.

```xml
<!-- enable query cache -->
<!-- hibernate.cfg.xml -->
<property name="cache.use_query_cache">true</property>
```

```java
// enable query cache for a query
Query.setCacheable(true)
``` 


#### How does Hibernate find an object?
1. Check whether this object is cached in the Session.
2. Check whether this object is cached in the SessionFactory.
3. Run a query to obtain that object.


### HQL vs SQL
```
- SQL:
SELECT s.name FROM Sample s WHERE s.age > 30;
		 |			 |    |			|
	   column      table alias    column

- HQL:
SELECT u.name FROM User as u where u.age > 30;
		 |			 |     |		|
       field      class   obj      field

-note: HQL does not support '*', we use 'from User' directly.
```

**note:** The translation of HQL is independent with mapping configuration.

### `load` vs `get()`
* if we use object as the key (which is a must for composite key), then for `load()`, it will return an new object as the result, in contrast, `get()` will fill in the object and return it.

### generator tag
class:
  1. assigned (default): assigned by user
  2. increment: max(id)+1
  3. sequence
  4. foreign
  5. native
  6. hilo

### Lazy Fetching
```java
class Node<E> {
	E value;
	Node<E> parent;
	Set<Node<E>> children;
}
```

By default, Hibernate will only load current object without loading any of its dependent objects (parent, children) unless we access them.

* Lazy Fetching can be disable by setting `lazy="false"` for its dependent sets.
* Lazy Fetching dependent objects can be forced to initialize by calling `Hibernate.initialize(object)`.

**note:** Lazy Fetching is different from Lazy Loading. Lazy Loading will not load current object until we access it.

### Transaction
A: Atomicity -
C: Consistency - 
I: Isolation -
D: Durability -

#### Isolation Levels
1. Read uncommitted (no lock) - may cause dirty read (read dirty data)
2. Read committed (prevent dirty read) (default level) - affected rows are locked
3. Repeatable Read (prevent non-repeatable read)
4. Serializable (prevent phantom read)

* dirty read: T2 update, T1 read before T2 commit. Because reading is faster than update, at some point, T1 will start reading non-updated data.

#### Pessimistic locking vs Optimistic locking
Optimistic locking: no lock, but we can add an indicator column (such as version), then we can use it to control reading without a lock.
