# endexam
// Project Structure
// src/main/java/com/klef/fsad/exam/Movie.java
// src/main/java/com/klef/fsad/exam/ClientDemo.java
// src/main/resources/hibernate.cfg.xml

// ======================= Movie.java =======================
package com.klef.fsad.exam;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

@Entity
@Table(name="movie")
public class Movie 
{
    @Id
    private int id;
    private String name;
    private String date;
    private String status;

    public Movie() {}

    public Movie(int id, String name, String date, String status) 
    {
        this.id = id;
        this.name = name;
        this.date = date;
        this.status = status;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }
}

// ======================= ClientDemo.java =======================
package com.klef.fsad.exam;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;
import org.hibernate.query.Query;

public class ClientDemo 
{
    public static void main(String[] args) 
    {
        Configuration cfg = new Configuration();
        cfg.configure("hibernate.cfg.xml");
        cfg.addAnnotatedClass(Movie.class);

        SessionFactory sf = cfg.buildSessionFactory();
        Session session = sf.openSession();

        // INSERT RECORD
        Transaction tx = session.beginTransaction();

        Movie m = new Movie(101, "Pushpa", "2024-01-10", "Released");
        session.persist(m);

        tx.commit();
        System.out.println("Record Inserted Successfully");

        // UPDATE USING HQL POSITIONAL PARAMETERS
        tx = session.beginTransaction();

        String hql = "update Movie set name=?1, status=?2 where id=?3";
        Query q = session.createQuery(hql);

        q.setParameter(1, "Pushpa 2");
        q.setParameter(2, "Upcoming");
        q.setParameter(3, 101);

        int n = q.executeUpdate();

        tx.commit();

        System.out.println(n + " Record Updated Successfully");

        session.close();
        sf.close();
    }
}

// ======================= hibernate.cfg.xml =======================
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
 <session-factory>

   <property name="hibernate.connection.driver_class">
      com.mysql.cj.jdbc.Driver
   </property>

   <property name="hibernate.connection.url">
      jdbc:mysql://localhost:3306/fsadendexam
   </property>

   <property name="hibernate.connection.username">
      root
   </property>

   <property name="hibernate.connection.password">
      root
   </property>

   <property name="hibernate.dialect">
      org.hibernate.dialect.MySQL8Dialect
   </property>

   <property name="hibernate.hbm2ddl.auto">update</property>
   <property name="hibernate.show_sql">true</property>

 </session-factory>
</hibernate-configuration>

// ======================= pom.xml =======================
<project xmlns="http://maven.apache.org/POM/4.0.0">
 <modelVersion>4.0.0</modelVersion>

 <groupId>com.klef.fsad</groupId>
 <artifactId>HibernateHQLDemo</artifactId>
 <version>1.0</version>

 <dependencies>

   <dependency>
     <groupId>org.hibernate.orm</groupId>
     <artifactId>hibernate-core</artifactId>
     <version>6.4.4.Final</version>
   </dependency>

   <dependency>
     <groupId>mysql</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>8.0.33</version>
   </dependency>

   <dependency>
     <groupId>jakarta.persistence</groupId>
     <artifactId>jakarta.persistence-api</artifactId>
     <version>3.1.0</version>
   </dependency>

 </dependencies>
</project>





// Project Structure
// src/main/java/com/klef/fsad/exam
// ├── FsadEndExamApplication.java
// ├── model/SupplierOrder.java
// ├── repository/SupplierOrderRepository.java
// ├── service/SupplierOrderService.java
// ├── controller/SupplierOrderController.java

// ===============================
// 1. FsadEndExamApplication.java
// ===============================
package com.klef.fsad.exam;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class FsadEndExamApplication {
    public static void main(String[] args) {
        SpringApplication.run(FsadEndExamApplication.class, args);
    }
}


// ===============================
// 2. model/SupplierOrder.java
// ===============================
package com.klef.fsad.exam.model;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

@Entity
@Table(name = "supplier_order")
public class SupplierOrder 
{
    @Id
    private int supplierOrderId;   // Manual ID

    private String name;
    private String date;
    private String status;
    private double amount;
    private String productName;

    // Getters and Setters
    public int getSupplierOrderId() {
        return supplierOrderId;
    }

    public void setSupplierOrderId(int supplierOrderId) {
        this.supplierOrderId = supplierOrderId;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }

    public double getAmount() {
        return amount;
    }

    public void setAmount(double amount) {
        this.amount = amount;
    }

    public String getProductName() {
        return productName;
    }

    public void setProductName(String productName) {
        this.productName = productName;
    }
}


// =======================================
// 3. repository/SupplierOrderRepository.java
// =======================================
package com.klef.fsad.exam.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.klef.fsad.exam.model.SupplierOrder;

public interface SupplierOrderRepository extends JpaRepository<SupplierOrder, Integer> 
{
}


// ===================================
// 4. service/SupplierOrderService.java
// ===================================
package com.klef.fsad.exam.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.klef.fsad.exam.model.SupplierOrder;
import com.klef.fsad.exam.repository.SupplierOrderRepository;

@Service
public class SupplierOrderService 
{
    @Autowired
    private SupplierOrderRepository repo;

    // Add SupplierOrder
    public SupplierOrder addOrder(SupplierOrder order) {
        return repo.save(order);
    }

    // Second GET Operation
    public List<SupplierOrder> viewAllOrders() {
        return repo.findAll();
    }
}


// =======================================
// 5. controller/SupplierOrderController.java
// =======================================
package com.klef.fsad.exam.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import com.klef.fsad.exam.model.SupplierOrder;
import com.klef.fsad.exam.service.SupplierOrderService;

@RestController
@RequestMapping("/supplierorder")
public class SupplierOrderController 
{
    @Autowired
    private SupplierOrderService service;

    // POST Request - Add SupplierOrder
    @PostMapping("/add")
    public SupplierOrder addOrder(@RequestBody SupplierOrder order) {
        return service.addOrder(order);
    }

    // GET Request - View All Orders
    @GetMapping("/viewall")
    public List<SupplierOrder> viewAllOrders() {
        return service.viewAllOrders();
    }
}


// ===============================
// 6. application.properties
// ===============================
spring.datasource.url=jdbc:mysql://localhost:3306/fsadendexam
spring.datasource.username=root
spring.datasource.password=yourpassword

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
server.port=8080


// ===============================
// POSTMAN TESTING
// ===============================

// 1. POST Request
// URL: http://localhost:8080/supplierorder/add

{
   "supplierOrderId":101,
   "name":"ABC Suppliers",
   "date":"2026-05-02",
   "status":"Pending",
   "amount":5000,
   "productName":"Laptops"
}


// 2. GET Request
// URL: http://localhost:8080/supplierorder/viewall
