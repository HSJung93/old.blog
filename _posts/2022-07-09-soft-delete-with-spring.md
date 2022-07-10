---
title: "Soft Delete with Spring JPA"
date: 2022-07-09T09:08:00+09:00
categories:
  - Memo
tags:
  - soft delete
  - Spring
  - JPA
# header:
#   teaser: /assets/images/code.jpg
---

## Soft Delete with Spring JPA

### Introduction

- Sometimes there are business requirements to not permanently delete data from the database.
- These requirements, for example, the need for data history tracking or audit and also related to reference integrity.
- Instead of physically deleting the data, we can just hide that data so that it can't be accessed from the application front-end.

### What Is Soft Delete?

- Soft delete performs an update process to mark some data as deleted instead of physically deleting it from a table in the database.
- A common way to implement soft delete is to add a field that will indicate whether data has been deleted or not.
- look at the SQL command we'll run when physically deleting a record from the table:

```sql
delete from table_product where id=1
```

- Note we added a new field called deleted. This field will contain the values 0 or 1.
- The value 1 will indicate the data has been deleted and 0 will indicate the data has not been deleted. 
- We should set 0 as the default value, and for every data deletion process, we don't run the SQL delete command, but the following SQL update command instead:

```sql
update from table_product set deleted=1 where id=1
```

- Using this SQL command we didn't actually delete the row, but only marked it as deleted. So, when we're going to perform a read query, and we only want those rows that have not been deleted, we should only add a filter in our SQL query:

```sql
select * from table_product where deleted=0
```

### How to Implement Soft Delete in Spring JPA

- With Spring JPA the implementation of soft delete has become much easier. We'll only need a few JPA annotations for this purpose.

#### Entity Class

- A Product entity class:

```java
@Entity
@Table(name = "table_product")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private double price;

    private boolean deleted = Boolean.FALSE;

    // setter getter methods
}
```

- Added a deleted property with the default value set as FALSE.
- The next step will be to override the delete command in the JPA repository.
- By default, the delete command in the JPA repository will run a SQL delete query, so let's first add some annotations to our entity class:

```java
@Entity
@Table(name = "table_product")
@SQLDelete(sql = "UPDATE table_product SET deleted = true WHERE id=?")
@Where(clause = "deleted=false")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private double price;

    private boolean deleted = Boolean.FALSE;
   
    // setter getter method
}
```

- using the `@SQLDelete` annotation to override the delete command.
- turned it into a SQL update command that changes the deleted field value to true
- `@Where` annotation will add a filter when we read the product data.

#### Repository

- no special changes in the repository class

#### Service

- Also for the service class, there is nothing special yet

### How to Get the Deleted Data?

- By using the @Where annotation, we can't get the deleted product data in case we still want the deleted data to be accessible.
- To implement this, we shouldn't use the `@Where` annotation but two different annotations, `@FilterDef`, and `@Filter`.
- `@FilterDef` annotation defines the basic requirements that will be used by `@Filter` annotation.

```java
@Entity
@Table(name = "tbl_products")
@SQLDelete(sql = "UPDATE tbl_products SET deleted = true WHERE id=?")
@FilterDef(name = "deletedProductFilter", parameters = @ParamDef(name = "isDeleted", type = "boolean"))
@Filter(name = "deletedProductFilter", condition = "deleted = :isDeleted")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private double price;

    private boolean deleted = Boolean.FALSE;
}
```

- also need to change the `findAll()` function in the `ProductService` service class to handle dynamic parameters or filters
- add the isDeleted parameter that we'll add to the object Filter affecting the process of reading the Product entity.

```java
@Service
public class ProductService {
    
    @Autowired
    private ProductRepository productRepository;

   <strong> @Autowired
    private EntityManager entityManager;</strong>

    public Product create(Product product) {
        return productRepository.save(product);
    }

    public void remove(Long id){
        productRepository.deleteById(id);
    }

    public Iterable<Product> findAll(<strong>boolean isDeleted</strong>){
       <strong> Session session = entityManager.unwrap(Session.class);
        Filter filter = session.enableFilter("deletedProductFilter");
        filter.setParameter("isDeleted", isDeleted);
        Iterable<Product> products =  productRepository.findAll();
        session.disableFilter("deletedProductFilter");
        return products;</strong>
    }
}
```
