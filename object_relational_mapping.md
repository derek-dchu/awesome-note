# Object Relational Mapping (ORM)
An ORM tool simplifies the data creation, data manipulation and data access. It is a programming technique that maps the object to the data stored in the database.

* Products:
  1. Hibernate
  2. Java Persistence API (JPA)
  3. EJB, EntityBean
  4. iBats
  5. TopLink

### Hibernate Session vs JPA EntityManager
| SQL | HS | EM |  
|-----|----|----|  
| insert | save(obj) | persist(obj) |  
| load an obj | load/get(<class>, id) | find(<class>, id) |  
| update | load/get + setter | find + setter |  
| delete | delete(obj) | remove(obj) |  
| select | createQuery(hql) | createQuery(JPAquery) |  
| get a result | query.list() | query.getResultList() |  

