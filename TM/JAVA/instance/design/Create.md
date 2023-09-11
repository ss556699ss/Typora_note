
# Create


## Static factory

**advantage**
1. exact name, is easier to read
2. Objects are not created every time they are called, and  object can be reused to control the number of object.

``` java
// there can only be one name and table construction content
Complex c = new Complex(1.0, 0.0);

// show the exact name
public static Complex fromRealNumber(double real) { 
	return new Complex(real, 0.0); 
}



Employee e1 = new Employee("001", "John"); Employee e2 = new Employee("001", "John");

public class Employee {
    private static Map<String, Employee> employees = new HashMap<>();

    public static Employee getInstance(String id, String name) {
        Employee existingEmployee = employees.get(id);
        if (existingEmployee == null) {
            existingEmployee = new Employee(id, name);
            employees.put(id, existingEmployee);
        }
        return existingEmployee;
    }

    private Employee(String id, String name) {
        // ...
    }
}


Employee e1 = Employee.getInstance("001", "John");
Employee e2 = Employee.getInstance("001", "John");

```



