# Principios SOLID en Spring Boot
 SOLID es un conjunto de principios de diseño de software que fueron introducidos por el ingeniero de software Robert C. Martin. Estos principios, junto con otros conceptos de diseño, buscan ayudar a los desarrolladores a crear código más limpio, modular y fácilmente mantenible.

La palabra "SOLID" es un acrónimo que representa los cinco principios individuales:

**1. Responsabilidad Única (SRP, Single Responsibility Principle):** 

Una clase debe tener una sola responsabilidad. Es decir, una clase debe tener un único motivo para cambiar. Esto promueve la cohesión y evita que una clase se vuelva demasiado grande y difícil de mantener.
En Spring Boot, podemos aplicar este principio creando clases que se encarguen de una única funcionalidad específica. Por ejemplo, consideremos una aplicación de gestión de usuarios donde tenemos una clase UserService que se encarga de las operaciones relacionadas con los usuarios:

```java
public class UserService {
    public User createUser(String username, String password) {
        // Lógica para crear un usuario en la base de datos
    }

    public void deleteUser(User user) {
        // Lógica para eliminar un usuario de la base de datos
    }

    public void updateUser(User user) {
        // Lógica para actualizar un usuario en la base de datos
    }
}
```
En este ejemplo, la clase UserService tiene una sola responsabilidad, que es gestionar las operaciones relacionadas con los usuarios.

**2. Abierto/Cerrado (OCP, Open/Closed Principle):** 

Las entidades de software deben estar abiertas para la extensión pero cerradas para la modificación. Esto implica que el comportamiento de una entidad debe poder ser extendido sin necesidad de modificar su código fuente original. En Spring Boot, podemos aplicar este principio utilizando la inyección de dependencias y la programación orientada a interfaces. Por ejemplo, consideremos una clase NotificationService que se encarga de enviar notificaciones:

```java
public interface NotificationSender {
    void sendNotification(String message);
}

public class EmailNotificationSender implements NotificationSender {
    public void sendNotification(String message) {
        // Lógica para enviar una notificación por correo electrónico
    }
}

public class SMSNotificationSender implements NotificationSender {
    public void sendNotification(String message) {
        // Lógica para enviar una notificación por SMS
    }
}

public class NotificationService {
    private NotificationSender notificationSender;

    public NotificationService(NotificationSender notificationSender) {
        this.notificationSender = notificationSender;
    }

    public void sendNotification(String message) {
        notificationSender.sendNotification(message);
    }
}
```
En este ejemplo, la clase NotificationService es abierta para la extensión, ya que se puede agregar fácilmente otro NotificationSender sin modificar la clase NotificationService. Esto nos permite introducir nuevos tipos de notificación sin cambiar el código existente.

**3. Sustitución de Liskov (LSP, Liskov Substitution Principle):** 

Las clases derivadas deben ser sustituibles por sus clases base sin afectar la integridad del sistema. En otras palabras, cualquier instancia de una clase base debe poder ser reemplazada por una instancia de una de sus subclases sin alterar el correcto funcionamiento del programa. En Spring Boot, podemos aplicar este principio al diseñar interfaces o clases base que sean coherentes y cohesivas. Por ejemplo, consideremos una interfaz Shape con el método calculateArea():

```java
public interface Shape {
    double calculateArea();
}

public class Rectangle implements Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double calculateArea() {
        return width * height;
    }
}

public class Circle implements Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```
En este ejemplo, tanto Rectangle como Circle implementan la interfaz Shape y pueden ser utilizados indistintamente en cualquier lugar donde se requiera un objeto Shape, cumpliendo así el principio de sustitución de Liskov.

**4. Segregación de Interfaces (ISP, Interface Segregation Principle):** 

Los clientes no deben verse obligados a depender de interfaces que no utilizan. En lugar de tener interfaces monolíticas, es preferible tener interfaces más pequeñas y específicas para que los clientes solo dependan de los métodos que necesitan. Por ejemplo, consideremos una interfaz Vehicle que tiene múltiples métodos:

```java
public interface Vehicle {
    void startEngine();
    void stopEngine();
    void accelerate();
    void brake();
}
```
En lugar de tener una única interfaz grande, podemos aplicar el principio ISP dividiéndola en interfaces más pequeñas y cohesivas, como EngineControllable y MotionControllable:

```java
public interface EngineControllable {
    void startEngine();
    void stopEngine();
}

public interface MotionControllable {
    void accelerate();
    void brake();
}
```
De esta manera, las clases que implementen Vehicle no se verán obligadas a implementar métodos que no necesitan.

**5. Inversión de Dependencia (DIP, Dependency Inversion Principle):** 

Los módulos de alto nivel no deben depender de módulos de bajo nivel, ambos deben depender de abstracciones. Este principio promueve la programación hacia interfaces, donde las clases de alto nivel dependen de interfaces en lugar de implementaciones concretas, lo que facilita la extensibilidad y el intercambio de componentes. Por ejemplo, consideremos una clase OrderService que depende de una implementación concreta de OrderRepository:

```java
public class OrderService {
    private OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public void createOrder(Order order) {
        orderRepository.save(order);
    }
}
```
Para aplicar el principio DIP, podemos definir una interfaz OrderRepository y hacer que OrderService dependa de dicha interfaz en lugar de una implementación concreta:

```java
public interface OrderRepository {
    void save(Order order);
}

public class JpaOrderRepository implements OrderRepository {
    public void save(Order order) {
        // Lógica para guardar el pedido en una base de datos JPA
    }
}

public class OrderService {
    private OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public void createOrder(Order order) {
        orderRepository.save(order);
    }
}
```
En este ejemplo, OrderService depende de la interfaz OrderRepository, lo que permite que se le inyecte cualquier implementación de OrderRepository sin afectar la lógica del servicio.
