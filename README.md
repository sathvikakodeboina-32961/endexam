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
