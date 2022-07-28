### 1. Create database named: FacultySystemDB.
> Database is created automatically when the next query is executed.
### 2. Create collection (student) that has: 
* FirstName: string
* LastName: string
* Age: Number
* Faculty: An object that has Name and Address
* Grades: An array of objects, each object has: CourseName, Grade, Pass (Boolean).
* IsFired: Boolean

```
use FacultySystemDB
db.createCollection("student", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         properties: {
            FirstName: {
               bsonType: "string"
            },
            LastName: {
               bsonType: "string"
            },
            Age: {
               bsonType: "int",
               minimum: 15,
               maximum: 120,
            },
            Faculty: {
                  bsonType: "object",
                  properties: {
                     FacultyName: {
                        bsonType: "string"
                     },
                     FacultyAddress: {
                        bsonType: "string"
                     }                     
                  }
            },
            Grades: {
               bsonType: ["array"],
               items: {
                  bsonType: "object",
                  required: ["CourseName", "Grade", "Pass"],
                  properties: {
                     CourseName: {
                        bsonType: "string"
                     },
                     Grade: {
                        bsonType: "string"
                     },
                     Pass: {
                        bsonType: "bool"
                     }
                  }
               }
            },

            IsFired: {
               bsonType: "bool"
            },
            }
         }
      }
   }
)
```

### 3. Insert 3 (at least) documents in Student collection with different values.
* Try inserting one record each time
```
db.student.insertOne({FirstName: "John", LastName: "Smith", Age: 24, Faculty: {FacultyName: "Engineering", FacultyAddress:"123 North St."}, Grades: [{CourseName: "Math 1", Grade: "48", Pass: false}, {CourseName:"Physics", Grade: "78", Pass:true}], IsFired: false})
```
```
db.student.insertOne({FirstName: "Mohamed", LastName: "Ahmed", Age: 26, Faculty: {FacultyName: "Computer Science", FacultyAddress:"124 North St."}, Grades: [{CourseName: "Math 2", Grade: "89", Pass: true}, {CourseName:"Architecture", Grade: "54", Pass:true}], IsFired: false})
```
```
db.student.insertOne({FirstName: "Thomas", LastName: "Edison", Age: 29, Faculty: {FacultyName: "Arts", FacultyAddress:"123 North St."}, Grades: [{CourseName: "English Literature", Grade: "25", Pass: false}, {CourseName:"Theatre", Grade: "32", Pass: false}], IsFired: true})
```


* Try inserting collection using one insert statement.
```
db.student.insertMany([
{FirstName: "John", LastName: "Smith", Age: 24, Faculty: {FacultyName: "Engineering", FacultyAddress:"123 North St."}, Grades: [{CourseName: "Math 1", Grade: "48", Pass: false}, {CourseName:"Physics", Grade: "78", Pass:true}], IsFired: false},
{FirstName: "Mohamed", LastName: "Ahmed", Age: 26, Faculty: {FacultyName: "Computer Science", FacultyAddress:"124 North St."}, Grades: [{CourseName: "Math 2", Grade: "89", Pass: true}, {CourseName:"Architecture", Grade: "54", Pass:true}], IsFired: false},
{FirstName: "Ahmed", LastName: "Mohamed", Age: 19, Faculty: {FacultyName: "Arts", FacultyAddress:"123 North St."}, Grades: [{CourseName: "English Literature", Grade: "25", Pass: false}, {CourseName:"Theatre", Grade: "32", Pass: false}], IsFired: true}])
```

### 4. Retrieve the following data:
* All Students.  
```db.student.find()```  

* Student with specific First Name.  
```db.student.find({FirstName: "Ahmed"})```  

* Students who his First Name=Ahmed, or Last Name= Ahmed.  
```db.student.find({$or: [{FirstName: "Ahmed"}, {LastName: "Ahmed"}]})```  

* Students that their First name isn't "Ahmed".  
```db.student.find({FirstName: {$neq: "Ahmed"}})```  

* Students with Age less than 21.  
```db.student.find({age: {$lt: 21}})```  

* All fired students.  
```db.student.find({IsFired: {$eq: true}})```  

* Students with Age more than or equal to 21, and their faculty isn't NULL.  
```db.student.find({$and: [ {Age: {$gte: 21} }, {Faculty : {$neq: NULL} } ] })```  

* Display student with specific First Name, and display his First Name, Last name, IsFired fields only.  
```db.student.find({FirstName: "Ahmed"}, {FirstName: 1, LastName: 1, IsFired: 1, _id: 0})```  


### 5. Update the student with specific FirstName, and change his LastName.
* Try Update() statement.  
```db.student.updateOne({FirstName: "Ahmed"}, {$set: {LastName: "Mahmoud"} })```
* Try Update() with Mulit option.
```db.student.updateMany({FirstName: "Ahmed"}, {$set: {LastName: "Mahmoud"} })```  
* Try Save().  
```db.collection.save() is deprecated according to the MongoDB documentation  https://www.mongodb.com/docs/v4.4/reference/method/db.collection.save/```

### 6. Delete Fired students.
```db.student.deleteMany({IsFired: {$eq: true}})```
### 7. Delete all students collection.
```db.student.drop()```
### 8. Delete the whole DB.
```db.dropDatabase()```  

### 9. Create new database named: FacultySystemV2.
* Create student collection that has (FirstName, LastName, IsFired, FacultyID, array of objects, each object has CourseID, grade).
```
db.use.FacultySystemV2
db.createCollection("student", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         properties: {
            FirstName: {
               bsonType: "string"
            },
            LastName: {
               bsonType: "string"
            },
            IsFired: {
               bsonType: "bool"
            },
            FacultyID: {
               bsonType: "int"
            },
            Courses: {
               bsonType: ["array"],
               items: {
                  bsonType: "object",
                  required: ["CourseID", "Grade"],
                  properties: {
                     CourseID: {
                        bsonType: "string"
                     },
                     Grade: {
                        bsonType: "string"
                     },
                  }
               }
            }
         }
      }
   }
})
```
* Create Faculty collection that has (Faculty Name, Address).  
```
db.createCollection("faculty", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         properties: {
            FacultyName: {
               bsonType: "string"
            },
            Address: {
               bsonType: "string"
            }
         }
      }
   }
})
```
* Create Course collection, which has (Course Name, Final Mark).  
```
db.createCollection("course", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         properties: {
            CourseName: {
               bsonType: "string"
            },
            FinalMark: {
               bsonType: "float"
            }
         }
      }
   }
})
```
* Insert some data in previous collections.

```db.student.insertOne({FirstName: "Jane", LastName: "Doe", IsFired: false, FacultyID: 123, Courses: [{CourseID: "321", Grade: "A"}, {CourseID: "789", Grade: "B"}]})```    

```db.faculty.insertOne({FacultyName: "Engineering", Address: "123 Giza St."})```    

```db.course.insertOne({CourseName: "Fluid Mechanics", FinalMark: 150.0})```
