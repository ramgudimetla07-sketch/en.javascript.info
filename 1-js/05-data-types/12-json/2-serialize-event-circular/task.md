// Java Program: Serialization with Circular Reference

import java.io.*;

// Employee class implements Serializable
class Employee implements Serializable {
    private static final long serialVersionUID = 1L;

    String name;
    Department department;

    Employee(String name) {
        this.name = name;
    }
}

// Department class implements Serializable
class Department implements Serializable {
    private static final long serialVersionUID = 1L;

    String deptName;
    Employee manager;

    Department(String deptName) {
        this.deptName = deptName;
    }
}

public class CircularSerializationDemo {

    public static void main(String[] args) {

        try {
            // Create objects
            Employee emp = new Employee("Ram");
            Department dept = new Department("Computer Science");

            // Circular reference
            emp.department = dept;
            dept.manager = emp;

            // Serialize object
            FileOutputStream fileOut =
                    new FileOutputStream("data.ser");
            ObjectOutputStream out =
                    new ObjectOutputStream(fileOut);

            out.writeObject(emp);

            out.close();
            fileOut.close();

            System.out.println("Object serialized successfully!");

            // Deserialize object
            FileInputStream fileIn =
                    new FileInputStream("data.ser");
            ObjectInputStream in =
                    new ObjectInputStream(fileIn);

            Employee deserializedEmp =
                    (Employee) in.readObject();

            in.close();
            fileIn.close();

            // Display data
            System.out.println("\nAfter Deserialization:");
            System.out.println("Employee Name: "
                    + deserializedEmp.name);

            System.out.println("Department: "
                    + deserializedEmp.department.deptName);

            System.out.println("Manager Name: "
                    + deserializedEmp.department.manager.name);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
importance: 5

---

# Exclude backreferences

In simple cases of circular references, we can exclude an offending property from serialization by its name.

But sometimes we can't just use the name, as it may be used both in circular references and normal properties. So we can check the property by its value.

Write `replacer` function to stringify everything, but remove properties that reference `meetup`:

```js run
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  occupiedBy: [{name: "John"}, {name: "Alice"}],
  place: room
};

*!*
// circular references
room.occupiedBy = meetup;
meetup.self = meetup;
*/!*

alert( JSON.stringify(meetup, function replacer(key, value) {
  /* your code */
}));

/* result should be:
{
  "title":"Conference",
  "occupiedBy":[{"name":"John"},{"name":"Alice"}],
  "place":{"number":23}
}
*/
```
